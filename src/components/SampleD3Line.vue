<template>
    <div class="sampleD3Line" :style="{
        'width': `${width}px`,
        'height': `${height}px`,
    }">
        <canvas
            ref="canvas"
            :width="`${width}px`"
            :height="`${height}px`">
        </canvas>
    </div>
</template>

<script>
import { max } from 'lodash';
import * as d3 from 'd3';

export default {
    name: 'SampleD3Line',
    props: {
        width: Number,
        height: Number,
        data: Array,
    },
    methods: {
        ctx () {
            return this.$refs.canvas.getContext('2d');
        },

        xAxis () {
            const context = this.ctx();
            const tickCount = 10;
            const tickSize = 6;
            const ticks = this.x.ticks(tickCount);
            const tickFormat = this.x.tickFormat();

            context.beginPath();
            ticks.forEach(d => {
                context.moveTo(this.x(d), this.innerHeight);
                context.lineTo(this.x(d), this.innerHeight + tickSize);
            });
            context.strokeStyle = "black";
            context.stroke();

            context.textAlign = "center";
            context.textBaseline = "top";
            ticks.forEach(d => {
                context.fillText(tickFormat(d), this.x(d), this.innerHeight + tickSize);
            });
        },
        yAxis () {
            const context = this.ctx();
            const tickCount = 10;
            const tickSize = 6;
            const tickPadding = 3;
            const ticks = this.y.ticks(tickCount);
            const tickFormat = this.y.tickFormat(tickCount);

            context.beginPath();
            ticks.forEach(d => {
                context.moveTo(0, this.y(d));
                context.lineTo(-6, this.y(d));
            });
            context.strokeStyle = "black";
            context.stroke();

            context.beginPath();
            context.moveTo(-tickSize, 0);
            context.lineTo(0.5, 0);
            context.lineTo(0.5, this.innerHeight);
            context.lineTo(-tickSize, this.innerHeight);
            context.strokeStyle = "black";
            context.stroke();

            context.textAlign = "right";
            context.textBaseline = "middle";
            ticks.forEach(d => {
                context.fillText(tickFormat(d), -tickSize - tickPadding, this.y(d));
            });

            context.save();
            context.rotate(-Math.PI / 2);
            context.textAlign = "right";
            context.textBaseline = "top";
            context.font = "bold 10px sans-serif";
            context.fillText("Price (US$)", -10, 10);
            context.restore();
        },

        async render () {
            const context = this.ctx();
            context.clearRect(0, 0, this.width, this.height);

            this.innerWidth = this.width - this.margin.left - this.margin.right,
            this.innerHeight = this.height - this.margin.top - this.margin.bottom;


            this.x = d3.scaleTime()
                .range([0, this.innerWidth]);

            this.y = d3.scaleLinear()
                .range([this.innerHeight, 0]);

            const line = d3.line()
                .x(d => this.x(d.x))
                .y(d => this.y(d.y))
                .curve(d3.curveStep)
                .context(context);

            this.x.domain(d3.extent(this.data, d => d.x));
            this.y.domain(d3.extent(this.data, d => d.y));

            this.xAxis();
            this.yAxis();

            context.beginPath();
            line(this.data);

            context.lineWidth = 1.5;
            context.strokeStyle = "steelblue";
            context.stroke();
        }
    },
    
    mounted () {
        const context = this.ctx();
        context.translate(this.margin.left, this.margin.top);
        this.render();
    },

    watch: {
        data: {
            deep: true,
            handler: function (params) {
                this.render ();
            }
        },
    },
    data () {
        return {
            margin: { top: 40, left: 40, right: 40, bottom: 40 },

            // --------------
            x: null,
            x: null,
            innerWidth: 0,
            innerHeight: 0,

        };
    }
}
</script>

<style lang="scss">
.sampleD3Line {
    display: block;
    overflow: hidden;
    border: 1px solid blue;

    canvas {
        width: 100%;
        height: 100%;
    }
}
</style>
