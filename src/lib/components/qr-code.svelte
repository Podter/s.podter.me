<script lang="ts">
	import QRCode from 'qrcode';
	import QrCodeIcon from 'lucide-svelte/icons/qr-code';
	import Download from 'lucide-svelte/icons/download';
	import { Button } from '$lib/components/ui/button';
	import * as Dialog from '$lib/components/ui/dialog';
	import * as Tooltip from '$lib/components/ui/tooltip/index';
	import UrlDisplay from './url-display.svelte';

	export let url: string;

	let qrCodeData = '';
	$: QRCode.toDataURL(url, { margin: 2, width: 512 }).then((data) => (qrCodeData = data));
</script>

<Dialog.Root>
	<Dialog.Trigger asChild let:builder={triggerBuilder}>
		<Tooltip.Root>
			<Tooltip.Trigger asChild let:builder={tooltipBuilder}>
				<Button variant="secondary" size="icon" builders={[triggerBuilder, tooltipBuilder]}>
					<QrCodeIcon size={18} />
					<span class="sr-only">Show QR code</span>
				</Button>
			</Tooltip.Trigger>
			<Tooltip.Content>
				<p>Show QR code</p>
			</Tooltip.Content>
		</Tooltip.Root>
	</Dialog.Trigger>
	<Dialog.Content class="sm:max-w-md">
		<Dialog.Header>
			<Dialog.Title>QR code</Dialog.Title>
			<Dialog.Description>
				This is the QR code for the shortened URL. You can scan it with your phone to visit the URL.
			</Dialog.Description>
		</Dialog.Header>
		<div class="my-1 flex w-full justify-center">
			<img src={qrCodeData} alt="QR code" class="aspect-square w-80" />
		</div>
		<div class="flex justify-center">
			<UrlDisplay {url} />
		</div>
		<Dialog.Footer>
			<Button href={qrCodeData} download>
				<span class="mr-2">Download</span>
				<Download size={16} />
			</Button>
		</Dialog.Footer>
	</Dialog.Content>
</Dialog.Root>
