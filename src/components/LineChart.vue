<template>
    <div>
        <canvas ref="lineChart"></canvas>
    </div>
</template>

<script>
import {
    Chart as ChartJS, Title, Tooltip,LineController, Legend, LineElement, PointElement, CategoryScale, LinearScale, Filler
} from 'chart.js';

ChartJS.register(Title, Tooltip,LineController, Legend, LineElement, PointElement, CategoryScale, LinearScale, Filler // Optional: for filling under the line
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
                            callbacks: {
                                label: function (context) {
                                    return `${context.dataset.label}: ${context.raw}`;
                                }
                            }
                        }
                    },
                    scales: {
                        x: {
                            title: {
                                display: true,
                                text: 'Currency'
                            }
                        },
                        y: {
                            title: {
                                display: true,
                                text: 'Exchange Rate'
                            },
                            beginAtZero: true
                        }
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