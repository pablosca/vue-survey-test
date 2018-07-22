<template>
    <div class="progress_container contain wider">
        <span class="text">
            <span class="current">{{ current + 1 }}</span> / <span class="max">{{ length }}</span>
        </span>
        <div class="progress">
            <div class="bar" :style="{ 'width': progressWidth }"></div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['current', 'length'],
    computed: {
        progressWidth () {
            return (((this.current + 1) / this.length) * 100) + '%';
        }
    }
}
</script>

