<template>
    <div class="home">
        <SampleD3Line
            :width="800"
            :height="400"
            :data="sampleData" />
    </div>
</template>

<script>
import SampleD3Line from '@/components/SampleD3Line';
export default {
    name: 'home',
    components: {
        SampleD3Line,
    },
    mounted () {
        document.title = 'D3 Spike';
    },
    data () {
        return {
            sampleData: [
                { x: '1790', y: 3.9 },
                { x: '1810', y: 7.2 },
                { x: '1830', y: 12.8 },
                { x: '1850', y: 23.1 },
                { x: '1870', y: 36.5 },
                { x: '1890', y: 62.9 },
                { x: '1910', y: 92.2 },
                { x: '1930', y: 123.2 },
                { x: '1950', y: 151.3 },
                { x: '1970', y: 203.2 },
                { x: '1990', y: 248.7 },
                { x: '2010', y: 308.7}
            ],
            sampleData2: [
                {
                    name: "E",
                    value: 0.12702,
                },
                {
                    name: "T",
                    value: 0.09056,
                },
                {
                    name: "A",
                    value: 0.08167,
                },
                {
                    name: "O",
                    value: 0.07507,
                },
                {
                    name: "I",
                    value: 0.06966,
                },
                {
                    name: "N",
                    value: 0.06749,
                },
                {
                    name: "S",
                    value: 0.06327,
                },
                {
                    name: "H",
                    value: 0.06094,
                },
                {
                    name: "R",
                    value: 0.05987,
                },
                {
                    name: "D",
                    value: 0.04253,
                },
                {
                    name: "L",
                    value: 0.04025
                },
                {
                    name: "C",
                    value: 0.02782
                },
                {
                    name: "U",
                    value: 0.02758
                },
                {
                    name: "M",
                    value: 0.02406
                },
                {
                    name: "W",
                    value: 0.0236
                },
                {
                    name: "F",
                    value: 0.02288
                },
                {
                    name: "G",
                    value: 0.02015
                },
                {
                    name: "Y",
                    value: 0.01974
                },
                {
                    name: "P",
                    value: 0.01929
                },
                {
                    name: "B",
                    value: 0.01492
                },
                {
                    name: "V",
                    value: 0.00978
                },
                {
                    name: "K",
                    value: 0.00772
                },
                {
                    name: "J",
                    value: 0.00153
                },
                {
                    name: "X",
                    value: 0.0015
                },
                {
                    name: "Q",
                    value: 0.00095
                },
                {
                    name: "Z",
                    value: 0.00074
                },
            ]
        }
    }
}
</script>

<style lang="scss">
.home {

}
</style>
