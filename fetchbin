import * as fs from 'fs';
import * as path from 'path';

const sources = [ // TODO: make this dynamic
	'https://raw.githubusercontent.com/sophb-chan/TaNDy-binaries/main',
];

async function downloadBinary(binary) {
	for (const [index, source] of sources.entries()) {
		try {
			const constructedURL = source.endsWith('/') ? source + binary : source + '/' + binary;

			const r = await fetch(constructedURL);
			if (r.status !== 200)
				throw new Error(`HTTP status not 200 (got ${r.status})`);
			
			const sourceCode = await r.text();
			console.log(`Hit (Source): (${index}) ${constructedURL}`);

			// TODO: install dependencies from pkg.dep file

			return sourceCode;
		} catch {
			console.log(`Miss: (${index}) ${source}`);
		}
	}

	throw new ReferenceError(`Binary "${binary}" not found in ${sources.length > 1 ? 'all ' : ''}${sources.length} source${sources.length === 1 ? '' : 's'}.`);
}

async function handle({
	args
} = {}) {
	if (args.length === 1) {
		console.log("Usage: fetchbin <binary name>");
		return;
	}

	const binPath = import.meta.dirname
	const binaryName = args[1];
	const binary = await downloadBinary(binaryName);

	console.log('\nWriting to disk...');
	const targetPath = path.join(import.meta.dirname, binaryName);
	console.log(targetPath);
	fs.writeFileSync(targetPath, binary);

	console.log(`\nInstalled "${binaryName}"!`);
}

export default handle;
