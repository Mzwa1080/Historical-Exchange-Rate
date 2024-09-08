<template>
    <div>
      <h1>Historical Exchange Rate Dashboard</h1>
      <DatePicker @date-changed="updateDateRange" />
      <div class="chart-container">
        <LineChart :data="chartData" />
      </div>
    </div>
  </template>
  
  <script>
  import DatePicker from '@/components/DatePicker.vue';
  import LineChart from '@/components/LineChart.vue';
  import axios from 'axios';
  import dayjs from 'dayjs'; // For date manipulation
  
  export default {
    name : 'HelloWorld',
    components: {
      DatePicker,
      LineChart
    },
    data() {
      return {
        chartData: {
          labels: [],
          datasets: []
        },
        currenciesToCompare: ['EUR', 'GBP', 'ZAR', ], // Define currencies to compare
        baseCurrency: 'USD' // Define base currency
      };
    },
    methods: {
      // Function to handle date range update
      updateDateRange(selectedDate) {
        this.fetchHistoricalExchangeRates(selectedDate, 7); // Fetch data for 7 days
      },
      async fetchHistoricalExchangeRates(endDate, days = 7) {
        // Calculate start date
        const startDate = dayjs(endDate).subtract(days - 1, 'day').format('YYYY-MM-DD');
  
        const url = `https://api.freecurrencyapi.com/v1/historical?apikey=fca_live_M8Cvv9WkYrKDJ4iQnmOSM0PbeJ9FTQr5tKVnXSSU&base_currency=${this.baseCurrency}&start_date=${startDate}&end_date=${endDate}`;
  
        try {
          const response = await axios.get(url);
          const data = response.data.data; // Adjust based on actual API response structure
  
          // Ensure data exists
          if (!data) {
            throw new Error('No data received from API');
          }
  
          // Extract dates and sort them
          const dates = Object.keys(data).sort();
  
          // Initialize chart labels and datasets
          const labels = dates;
          const datasets = this.currenciesToCompare.map(currency => ({
            label: currency,
            borderColor: this.getRandomColor(),
            data: dates.map(date => data[date][currency] || 0),
            fill: false
          }));
  
          // Update chart data
          this.chartData = {
            labels,
            datasets
          };
        } catch (error) {
          console.error('Error fetching exchange rates:', error);
          alert('Failed to fetch exchange rates. Please try again later.');
        }
      },
      getRandomColor() {
        // Generate a random color in hex format
        return '#' + Math.floor(Math.random() * 16777215).toString(16).padStart(6, '0');
      }
    },
    mounted() {
      // Initialize with today's date
    
  // Static test data
//   this.chartData = {
//     labels: ['2024-09-01', '2024-09-02', '2024-09-03'],
//     datasets: [
//       {
//         label: 'EUR',
//         borderColor: '#FF0000',
//         data: [1.2, 1.3, 1.25],
//         fill: false
//       },
//       {
//         label: 'GBP',
//         borderColor: '#0000FF',
//         data: [0.85, 0.86, 0.87],
//         fill: false
//       }
//     ]
//   };


      const today = dayjs().format('YYYY-MM-DD');
      this.fetchHistoricalExchangeRates(today, 7);
    }
  };
  </script>
  
  <style scoped>
  .chart-container {
    position: relative;
    height: 500px;
    width: 100%;
  }
  </style>
  