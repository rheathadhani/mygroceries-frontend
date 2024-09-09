<template>
    <div class="card shadow-sm mb-4"> <!-- Add Bootstrap card class -->
      <div class="card-header bg-dark text-white text-center">
        Product Performance
      </div>
      <div class="card-body"> <!-- Card body for the chart -->
        <canvas id="productPerformanceChart"></canvas>
      </div>
    </div>
  </template>
  
  <script>
  import { Chart, registerables } from 'chart.js';
  
  // Register the necessary components
  Chart.register(...registerables);
  
  export default {
    name: 'ProductPerformanceChart',
    mounted() {
      this.createChart();
    },
    methods: {
      createChart() {
        const ctx = document.getElementById('productPerformanceChart').getContext('2d');
  
        new Chart(ctx, {
          type: 'bar', // Specify the type of chart
          data: {
            labels: ['Product A', 'Product B', 'Product C', 'Product D', 'Product E'], // Product names
            datasets: [
              {
                label: 'Sales',
                data: [50, 75, 100, 60, 90], // Example sales data for each product
                backgroundColor: [
                  'rgba(54, 162, 235, 0.8)', // Blue shades
                  'rgba(75, 192, 192, 0.8)',
                  'rgba(153, 102, 255, 0.8)',
                  'rgba(255, 159, 64, 0.8)',
                  'rgba(54, 162, 235, 0.8)',
                ],
                borderColor: [
                  'rgba(54, 162, 235, 1)', // Blue shades for border
                  'rgba(75, 192, 192, 1)',
                  'rgba(153, 102, 255, 1)',
                  'rgba(255, 159, 64, 1)',
                  'rgba(54, 162, 235, 1)',
                ],
                borderWidth: 1,
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
                  text: 'Products',
                },
              },
              y: {
                display: true,
                beginAtZero: true,
                title: {
                  display: true,
                  text: 'Sales Figures',
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
  #productPerformanceChart {
    max-width: 100%;
    height: 300px;
  }
  </style>
  