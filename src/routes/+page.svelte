<script lang="ts">
	import { parseBlob } from 'music-metadata';

	let files: FileList | null = $state(null);
	let title: string = $state('');
	let artist: string = $state('');
	let album: string = $state('');

	async function parseAudio(): Promise<void> {
		const fileInput: HTMLInputElement | null = document.querySelector('input[type="file"]');
		if (fileInput === null || fileInput.files === null) return;
		files = fileInput.files;
		const file = files[0];

		try {
			const metadata = await parseBlob(file);
			console.log(metadata);
			if (metadata.common.title !== undefined) {
				title = metadata.common.title;
			}
			if (metadata.common.artist !== undefined) {
				artist = metadata.common.artist;
			}
			if (metadata.common.album !== undefined) {
				album = metadata.common.album;
			}
		} catch (error) {
			console.error('Error parsing metadata: ', error);
		}
	}
</script>

<h1>Melodata</h1>
<label for="song">select an audio file</label>
<div>
	<input type="file" id="song" name="song" accept="audio/*" />
	<button onclick={parseAudio}>parse</button>
</div>
<div>
	<div><label for="title">title</label><input type="text" bind:value={title} id="title" /></div>
	<div><label for="artist">artist</label><input type="text" bind:value={artist} id="artist" /></div>
	<div><label for="album">album</label><input type="text" bind:value={album} id="album" /></div>
</div>
