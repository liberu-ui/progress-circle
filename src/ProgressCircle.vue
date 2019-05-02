<template>
    <svg class="progress-circle"
        :width="width"
        viewBox="0 0 20 20">
        <circle class="background"
            :style="backgroundStyle"
            cx="10" cy="10" r="8"/>
        <circle class="progress"
            cx="10" cy="10" r="8"
            :style="progressStyle"/>
        <text x="50%" y="58%"
            :style="textStyle">
            <slot name="content">
                {{ tweenedProgress.toFixed(0) }}%
            </slot>
        </text>
    </svg>
</template>

<script>
import TweenLite from 'gsap/TweenLite';

export default {
    name: 'ProgressCircle',

    props: {
        progress: {
            type: Number,
            required: true,
            validator: v => v >= 0 && v <= 100,
        },
        backgroundStroke: {
            type: String,
            default: '#f1f1f1',
        },
        progressStroke: {
            type: String,
            default: '#2ecc71',
        },
        textColor: {
            type: String,
            default: '#555',
        },
        width: {
            type: Number,
            required: true,
        },
    },

    data: () => ({
        tweenedProgress: 0,
    }),

    computed: {
        limit() {
            return -51 - ((51 / 100) * parseFloat(this.tweenedProgress));
        },
        progressStyle() {
            return {
                strokeDashoffset: `${this.limit}px`,
                stroke: this.progressStroke,
            };
        },
        backgroundStyle() {
            return {
                stroke: this.backgroundStroke,
            };
        },
        textStyle() {
            return {
                fill: this.textColor,
            };
        },
    },

    watch: {
        progress: {
            immediate: true,
            handler(progress) {
                TweenLite.to(this.$data, 0.2, { tweenedProgress: progress });
            },
        },
    },
};
</script>

<style lang="scss">
    .progress-circle {
        circle.background {
            fill: none;
            stroke-width: 1;
        }

        circle.progress {
            fill: none;
            stroke-width: 1.5;
            stroke-dasharray: 51 51;
            stroke-dashoffset: -51;
            stroke-linecap: round;
        }

        text {
            font-size: 5px;
            text-anchor: middle;
        }
    }
</style>
