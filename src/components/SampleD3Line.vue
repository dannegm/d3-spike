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
import d3 from 'd3';

export default {
    name: 'SampleD3Line',
    props: {
        width: Number,
        height: Number,
        data: Array,
    },
    methods: {
        canvas () {
            return  d3.select(this.$refs.canvas);
        },

        getMax (axis) {
            return max (
                this.data.map (i => i[axis])
            );
        },
        getInc (axis) {
            return Math.round (
                this.getMax(axis) / this.data.length
            ) - 1;
        },

        renderLinesAndLabels () {
            const yInc = this.getInc('y');
            const xInc = this.getInc('x');

            let xPos = 0;
            let yPos = 0;

            this.data.forEach((item, index) => {
                yPos += (index == 0) ? 0 : yInc;
                this.drawLine (
                    { x: 0, y: yPos },
                    { x: this.getMax('x'), y: yPos },
                    '#E8E8E8'
                );
                
                this.drawText (yPos, {
                    x: 10,
                    y: yPos + 4
                });
                this.drawText (item.x, {
                    x: xPos,
                    y: this.getMax('y')
                });
                
                xPos += xInc;
                
                this.drawLine (
                    { x: 0, y: 0 },
                    { x: 0, y: this.getMax('y') }
                );
                this.drawLine (
                    { x: 0, y: this.getMax('y') },
                    { x: this.getMax('x'), y: this.getMax('y') }
                );
            });
        },

        renderData () {
            let prevX = 0;
            let prevY = 0;

            this.data.forEach((item, index) => {
                const x = index * this.getInc('x');
                const y = (this.getMax('y') - item.y) / this.ratio;

                this.drawLine (
                    { x: x, y: y },
                    { x: prevX, y: prevY },
                    'black', 2
                );
                
                prevX = x;
                prevY = y;
            });
        },

        render () {
            this.ratio = this.height / this.getMax('y');
            this.ctx = this.$refs.canvas.getContext('2d');

            this.renderLinesAndLabels ();
            this.renderData ();
        },

        drawLine (start, end, strokeStyle = 'black', lineWidth = 1) {
            this.ctx.strokeStyle = strokeStyle;
            this.ctx.lineWidth = lineWidth;

            this.ctx.beginPath();
            this.ctx.moveTo(start.x, start.y);
            this.ctx.lineTo(end.x, end.y);
            this.ctx.stroke();
            this.ctx.closePath();
        },

        drawText (text, pos) {
            let txtSize = this.ctx.measureText(text);
            this.ctx.font = this.dataPointFont;
            this.ctx.fillText(text, pos.x, pos.y);
        }
    },

    mounted () {
        this.render();
    },
    data () {
        return {
            title: 'US Population Chart',
            xLabel: 'Year',
            yLabel: 'Population (millions)',
            labelFont: '19pt Arial',
            dataPointFont: '10pt Arial',

            // --------------
            ctx: null,
            ratio: null,
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
