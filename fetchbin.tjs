import * as fs from 'fs';
import * as path from 'path';

const sources = [ // TODO: make this dynamic somehow
	'https://raw.githubusercontent.com/sophb-chan/TaNDy-binaries/main',
];

async function downloadBinary(binary) {
	for (const [index, source] of sources.entries()) {
		const constructedURL =
			source.includes('<$BINARY>')
				? source.replaceAll('<$BINARY>', binary)
				: source.endsWith('/') ? source + binary : source + '/' + binary;
		try {
			const r = await fetch(constructedURL);
			if (r.status !== 200)
				throw new Error(`HTTP status not 200 (got ${r.status})`);

			const sourceCode = await r.text();
			console.log(`Hit (Source): (${index}) ${constructedURL}`);

			// TODO: install dependencies from pkg.dep file

			return sourceCode;
		} catch {
			console.log(`Miss: (${index}) ${constructedURL}`);
		}
	}

	throw new ReferenceError(`Binary "${binary}" not found in ${sources.length > 1 ? 'all ' : ''}${sources.length} source${sources.length === 1 ? '' : 's'}.`);
}

async function handle({
	args,
	validBinExtensions,
	reloadBinaries,
} = {}) {
	if (args.length === 1) {
		console.log("Usage: fetchbin <binary names>");
		return;
	}

	const binPath = import.meta.dirname
	const binaryNames = args.slice(1);
	for (const binaryName of binaryNames) {
		try {
			console.log(`Installing '${binaryName}'...\n`);
			let binary, binaryFilename;
			for (const [index, extension] of validBinExtensions.entries()) {
				const filename = binaryName + extension;
				const attemptDownload = async () => void (binary = await downloadBinary(filename));

				if (index === validBinExtensions.length - 1) await attemptDownload();
				else {
					try {
						await attemptDownload();
						binaryFilename = filename;
						break;
					} catch (err) {
						// Ignore errors about missing binaries
						if (err.message == null) continue;
						const tests = [/^Binary "/];
						if (!tests.every(test => test instanceof RegExp ? test.test(err.message) : err.message.includes(test))) throw err;
					}
				}
			}

			const targetPath = path.join(import.meta.dirname, binaryFilename);
			console.log(`\nWriting to disk ("${targetPath}")...`);
			fs.writeFileSync(targetPath, binary);

			console.log(`\nInstalled "${binaryName}"!`);
		} catch {
			console.log(`Binary "${binaryName}" not found in ${sources.length > 1 ? 'all ' : ''}${sources.length} source${sources.length === 1 ? '' : 's'}.`);
		}
	}
	console.log('Reloading binaries...');
	reloadBinaries();
}

export default handle;
