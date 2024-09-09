<template>
    <div class="card shadow-sm mb-4"> <!-- Add Bootstrap card class -->
      <div class="card-header bg-dark text-white text-center">
        Sales Over Time
      </div>
      <div class="card-body"> <!-- Card body for the chart -->
        <canvas id="salesChart"></canvas>
      </div>
    </div>
  </template>
  
  <script>
  import { Chart, registerables } from 'chart.js';
  
  // Register the necessary components
  Chart.register(...registerables);
  
  export default {
    name: 'SalesOverTimeChart',
    mounted() {
      this.createChart();
    },
    methods: {
      createChart() {
        const ctx = document.getElementById('salesChart').getContext('2d');
  
        new Chart(ctx, {
          type: 'line', // Specify the type of chart
          data: {
            labels: ['Day 1', 'Day 2', 'Day 3', 'Day 4', 'Day 5', 'Day 6', 'Day 7'],
            datasets: [
              {
                label: 'Sales Over Time',
                data: [12, 19, 3, 5, 2, 3, 9], // Example data
                borderColor: 'rgba(54, 162, 235, 1)', // Blue color for the line
                backgroundColor: 'rgba(54, 162, 235, 0.2)', // Light blue for fill
                borderWidth: 2,
                fill: true, // Fill under the line
              },
            ],
          },
          options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
              x: {
                display: true,
                title: {
                  display: true,
                  text: 'Days',
                },
              },
              y: {
                display: true,
                title: {
                  display: true,
                  text: 'Sales',
                },
              },
            },
          },
        });
      },
    },
  };
  </script>
  
  <style scoped>
  #salesChart {
    max-width: 100%;
    height: 300px;
  }
  </style>
  