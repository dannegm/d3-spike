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
    name: 'SampleCanvasLine',
    props: {
        width: Number,
        height: Number,
        data: Array,
    },
    methods: {
        canvas () {
            return  d3.select(this.$refs.canvas);
        },
        attr (attr) {
            return parseInt(this.$refs.canvas.getAttribute (attr));
        },

        getMax (axis) {
            return max (
                this.data.map (i => i[axis])
            );
        },
        getInc (val) {
            return Math.round (
                val / this.data.length
            ) - 1;
        },

        renderLinesAndLabels () {
            const yInc = this.getInc(this.yMax);
            const xInc = this.getInc(this.xMax) - 1;

            let xPos = this.margin.left;
            let yPos = 0;

            this.data.forEach((item, index) => {
                yPos += (index == 0) ? this.margin.top : yInc;
                this.drawLine (
                    { x: this.margin.left, y: yPos },
                    { x: this.xMax, y: yPos },
                    '#E8E8E8'
                );
                
                const yLabel = Math.round(
                    this.getMax('y') - ((index == 0) ? 0 :
                    yPos / this.ratio)
                );

                this.drawText (yLabel, {
                    x: this.margin.left,
                    y: yPos + 4
                });
                this.drawText (item.x, {
                    x: xPos,
                    y: this.yMax + (this.margin.bottom / 3)
                });
                
                xPos += xInc;
                
                this.drawLine (
                    { x: this.margin.left, y: this.margin.top },
                    { x: this.margin.left, y: this.yMax }
                );
                this.drawLine (
                    { x: this.margin.left, y: this.yMax },
                    { x: this.xMax, y: this.yMax }
                );
            });
        },

        renderData () {
            const yInc = this.getInc(this.yMax);
            const xInc = this.getInc(this.xMax) - 1;

            let prevX = 0;
            let prevY = 0;

            this.data.forEach((item, index) => {
                let x = (index * xInc) + this.margin.left;
                let y = (this.getMax('y') - item.y)  * this.ratio;

                if (y < this.margin.top) y = this.margin.top;
                if (index > 0) {
                    this.drawLine (
                        { x: x, y: y },
                        { x: prevX, y: prevY },
                        'black', 2
                    );
                    this.drawBullet ({ x, y });
                }
                
                prevX = x;
                prevY = y;
            });
        },

        render () {
            this.ctx = this.$refs.canvas.getContext('2d');

            this.xMax = this.attr ('width') - (this.margin.left + this.margin.right);
            this.yMax = this.attr ('height') - (this.margin.top + this.margin.bottom);
            this.ratio = this.yMax / this.getMax('y');

            this.ctx.clearRect(0, 0, this.width, this.height);
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

        drawBullet (pos) {
            this.ctx.beginPath();
            this.ctx.arc(pos.x, pos.y, 6, 0, 2 * Math.PI, false);
            this.ctx.fillStyle = 'red';
            this.ctx.fill();
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
            dataPointFont: '8pt Arial',
            margin: { top: 40, left: 40, right: 40, bottom: 40 },

            // --------------
            xMax: null,
            yMax: null,
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
