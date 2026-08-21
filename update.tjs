import * as fs from 'fs';
import * as path from 'path';

async function fetchFromRemote(filepath) {
	const remoteSrc = 'https://raw.githubusercontent.com/sophb-chan/TaNDy/main/';
	const r = await fetch(remoteSrc + filepath);
	return await r.text();
}
async function handle({
	tandyDir
} = {}) {
	// Conversion table between local and remote filenames
	const nametable = {
		'term.js': 'term.js',
		'minimist-string.js': 'minimist-string.js',
		'tandy': 'tandy.js',
	};

	// Contents of remotely-stored files
	const remoteContents = {};
	// List files that need to be updated
	const needsUpdate = {};
	for (const [localName, remoteName] of Object.entries(nametable)) {
		console.log(`Checking '${localName}'...`);
		const localContent = fs.readFileSync(path.join(tandyDir, localName), { encoding: 'utf-8' });
		const remoteContent = await fetchFromRemote(remoteName);
		remoteContents[remoteName] = remoteContent;
		const upToDate = localContent === remoteContent;
		needsUpdate[localName] = !upToDate;
	}

	// Update files that need to be updated
	if (Object.values(needsUpdate).some(Boolean)) {
		for (const [localName, updateNeeded] of Object.entries(needsUpdate)) {
			if (!updateNeeded) continue;
			console.log(`Updating '${localName}'...`);
			const newContent = remoteContents[nametable[localName]];
			fs.writeFileSync(path.join(tandyDir, localName), newContent);
		}
		console.log("Updated successfully!");
	} else {
		console.log('Your installation of TaNDy is already up-to-date.');
	}
}

export default handle;
