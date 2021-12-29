<template>
    <div>
        <div v-for="song in items" :key="song.id">
            {{ song.title }} = {{ song.id}} points
        </div>
    </div>
</template>

<script>
    export default {
        name: "Carousel",
        props: [
            "items"
        ]
    }

    
</script>