<template>
    <div>
        <canvas ref="lineChart"></canvas>
    </div>
</template>

<script>
import {
    Chart as ChartJS, Title,   TimeScale,  Tooltip, LineController, Legend, LineElement, PointElement, CategoryScale, LinearScale, Filler
} from 'chart.js';

import 'chartjs-adapter-date-fns';


ChartJS.register(Title, Tooltip , TimeScale, LineController, Legend, LineElement, PointElement, CategoryScale, LinearScale, Filler // Optional: for filling under the line
);



export default {
    props: {
        data: {
            type: Object,
            required: true,
            default: () => ({
                labels: [],
                datasets: []
            })
        }
    },
    mounted() {
        this.renderChart(this.data);
    },
    watch: {
        data: {
            handler(newData) {
                this.renderChart(newData);
            },
            deep: true
        }
    },
    methods: {
    renderChart(data) {
        const ctx = this.$refs.lineChart.getContext('2d');
        if (this.chart) {
            this.chart.destroy(); // Destroy previous chart instance to prevent duplication
        }
        this.chart = new ChartJS(ctx, {
            type: 'line',
            data: {
                labels: data.labels,
                datasets: data.datasets
            },
            options: {
                responsive: true,
                plugins: {
                    legend: {
                        display: true,
                        position: 'top'
                    },
                    tooltip: {
                        mode: 'nearest',
                        intersect: false,
                        callbacks: {
                            label: function (context) {
                                return `${context.dataset.label}: ${context.raw}`;
                            }
                        }
                    }
                },
                interaction: {
                    mode: 'nearest',
                    axis: 'x',
                    intersect: false,
                },
                hover: {
                    mode: 'nearest',
                    intersect: false,
                },
                elements: {
                    point: {
                        radius: 5,
                        hoverRadius: 8,
                        backgroundColor: 'rgba(75, 192, 192, 1)',
                    },
                    line: {
                        borderWidth: 2,
                        tension: 0.4,
                        fill: false,
                    },
                },
                scales: {
                    x: {
                        type: 'time',
                        time: {
                            unit: 'hour',
                            tooltipFormat: 'HH:mm' // Format for tooltip
                        },
                        title: {
                            display: true,
                            text: 'Time'
                        },
                        ticks: {
                            autoSkip: true,
                            maxRotation: 0,
                            minRotation: 0,
                            padding: 10
                        },
                        grid: {
                            display: false,
                        }
                    },
                    y: {
                        title: {
                            display: true,
                            text: 'Exchange Rate'
                        },
                        beginAtZero: true
                    }
                },
                animation: {
                    duration: 1000,
                    easing: 'easeOutQuart',
                }
            }
        });
    }
}



};
</script>

<style scoped>
/* Add your styles here */
</style>