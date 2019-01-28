<template>
    <div class="sampleD3Bar" :style="{
        'width': width,
        'height': height,
    }">
        <svg ref="canvas"></svg>
    </div>
</template>

<script>
import d3 from "d3";

export default {
    name: 'SampleD3Bar',
    props: {
        width: String,
        height: String,
        data: Array,
    },
    methods: {
        canvas () {
            return  d3.select(this.$refs.canvas);
        },
        

        chart () {
        }
    }
}
</script>

<style lang="scss">
.sampleD3Bar {
    display: block;
    overflow: hidden;
    border: 1px solid red;
}
</style>
