import * as path from 'path';
import * as os from 'os';

function handle({
	args
}) {
	if (args.length === 1) {
		console.log("Please specify a directory to go to.");
		return;
	}

	let targetDir = args[1];
	if (targetDir === '~') {
		targetDir = os.homedir();
	} else if (targetDir.startsWith('~/')) {
		targetDir = path.join(os.homedir(), targetDir.slice(2));
	}

	try {
		process.chdir(targetDir);
	} catch (err) {
		console.error(`Unable to go to ${path.join(import.meta.dirname, args[1])}:`);
		throw err;
	}
}

export default handle;
