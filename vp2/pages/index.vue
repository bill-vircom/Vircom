<script setup lang="ts">
import { useTheme } from 'vuetify'
import { hexToRgb } from '@layouts/utils'

const moreList = [
  { title: 'Last 28 Days', value: 'Last 28 Days' },
  { title: 'Last Month', value: 'Last Month' },
  { title: 'Last Year', value: 'Last Year' },
]

const vuetifyTheme = useTheme()

const chartConfig = computed(() => {
  const themeColors = vuetifyTheme.current.value.colors
  const variableTheme = vuetifyTheme.current.value.variables

  const secondaryText = `rgba(${hexToRgb(String(themeColors['on-background']))},${variableTheme['medium-emphasis-opacity']})`
  const borderColor = `rgba(${hexToRgb(String(variableTheme['border-color']))},${variableTheme['border-opacity']})`
  const disabledText = `rgba(${hexToRgb(String(themeColors['on-background']))},${variableTheme['disabled-opacity']})`

  return {
    chart: {
      parentHeightOffset: 0,
      toolbar: { show: false },
    },
    legend: {
      fontSize: '15px',
      markers: {
        width: 10,
        height: 10,
        offsetX: -2,
      },
      itemMargin: { horizontal: 10 },
      labels: { colors: secondaryText },
    },
    plotOptions: {
      radar: {
        size: 100,
        polygons: {
          strokeColors: borderColor,
          connectorColors: borderColor,
        },
      },
    },
    fill: {
      type: 'gradient',
      gradient: {
        shade: 'dark',
        gradientToColors: [themeColors.warning, themeColors.primary],
        shadeIntensity: 1,
        type: 'vertical',
        opacityFrom: 1,
        opacityTo: 0.9,
        stops: [0, 100],
      },
    },
    colors: [themeColors.warning, themeColors.primary],
    labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
    markers: { size: 0 },
    xaxis: {
      labels: {
        show: true,
        style: {
          fontSize: '14px',
          colors: [
            disabledText,
            disabledText,
            disabledText,
            disabledText,
            disabledText,
            disabledText,
          ],
        },
      },
    },
    yaxis: { show: false },
    grid: { show: false },
  }
})

const series = [
  {
    name: 'Income',
    data: [70, 90, 80, 95, 75, 90],
  },
  {
    name: 'Net Worth',
    data: [110, 72, 62, 65, 100, 75],
  },
]
</script>

<template>
  <div>
    <VCard elevation="0" class="mb-6">
      <VCardText>{{ $t("hello_world") }}</VCardText>
      <VCardText>
        <VTextField
          label="Regular"
          placeholder="Placeholder Text"
        />
      </VCardText>
    </VCard>
  
    <VCard elevation="0" class="mb-6">
      <VCardText>
        <VueApexCharts
          type="radar"
          height="280"
          :options="chartConfig"
          :series="series"
        />
      </VCardText>
    </VCard>
  </div>
</template>
