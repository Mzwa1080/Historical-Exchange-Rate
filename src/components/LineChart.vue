<template>
    <div>
        <canvas ref="lineChart"></canvas>
    </div>
</template>

<script>
import {
    Chart as ChartJS, Title, Tooltip, LineController, Legend, LineElement, PointElement, CategoryScale, LinearScale, Filler
} from 'chart.js';

ChartJS.register(Title, Tooltip, LineController, Legend, LineElement, PointElement, CategoryScale, LinearScale, Filler // Optional: for filling under the line
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
                            mode: 'nearest', // Tooltip will show nearest point
                            intersect: false,
                            callbacks: {
                                label: function (context) {
                                    return `${context.dataset.label}: ${context.raw}`;
                                }
                            }
                        }
                    },
                    interaction: {
                        mode: 'nearest', // Interaction with nearest points and lines
                        axis: 'x', // Restrict movement to the x-axis (horizontal)
                        intersect: false, // Points and lines will react without intersecting
                    },
                    hover: {
                        mode: 'nearest',
                        intersect: false,
                    },
                    elements: {
                        point: {
                            radius: 5, // Base radius of the points
                            hoverRadius: 8, // Larger size on hover
                            backgroundColor: 'rgba(75, 192, 192, 1)', // Point color
                        },
                        line: {
                            borderWidth: 2, // Width of the line
                            tension: 0.4, // Smoothness of the line (curved line)
                            fill: false, // Disable fill under the line
                        },
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
                    },
                    animation: {
                        duration: 1000, // Animation duration for smooth transitions
                        easing: 'easeOutQuart', // Easing function for a smoother effect
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