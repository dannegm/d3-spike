<template>
    <div class="home">
        <div class="left">
            <SampleD3Line
                :width="chartLine.width"
                :height="chartLine.height"
                :data="chartLine.data" />
        </div>
        <div class="right">
            <ul>
                <li v-for="(item, index) in chartLine.data" :key="index">
                    <input type="range" min="0" max="400" value="90" step="1"
                        v-model="item.y">
                    <label>
                        <b>x:</b> {{item.x}}, <b>y:</b> {{item.y}}
                    </label>
                </li>
            </ul>
        </div>
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
            chartLine: {
                width: 800,
                height: 600,
                data: [
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
            },
        }
    }
}
</script>

<style lang="scss">
.home {
    display: flex;
    flex-direction: row;
    
    .left {
        width: 800px;
    }
}
</style>
