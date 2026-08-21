async function handle({
	args
} = {}) {
	if (args.length === 1)
		throw new Error("Need durations to sleep for - Usage: sleep [duration 1] [duration 2] ... [duration N]");

	const durations = args.slice(1).map(d => parseInt(d) * 1000).filter(d => !isNaN(d));
	const delay = ms => new Promise(r => setTimeout(r, ms));
	for (const duration of durations) await delay(duration);
}

export default handle;
