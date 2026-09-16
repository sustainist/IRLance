<script>
    let loaded = $state(false);
</script>

<div class="cover">
    <img
        class:loaded
        src="/cover.png"
        alt="IRLance book cover"
        onload={() => (loaded = true)}
    />
</div>

<style>
    .cover {
        display: flex;
        justify-content: center;
        margin: 0 auto;
    }

    img {
        border-radius: 0.5rem;
        display: block;
        aspect-ratio: auto;
        filter: blur(18px);
        max-width: 100%;
        max-height: calc(100vh - 24px);
        transition: filter 400ms ease;
        width: auto;
        height: auto;
    }

    img.loaded {
        filter: blur(0);
    }
</style>
