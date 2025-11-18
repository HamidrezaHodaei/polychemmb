<template>
  <div class="world-map-wrapper">
    <div ref="chartdiv" class="chartdiv"></div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import * as am5 from '@amcharts/amcharts5'
import * as am5map from '@amcharts/amcharts5/map'
import am5geodata_worldLow from '@amcharts/amcharts5-geodata/worldLow'

const chartdiv = ref<HTMLElement | null>(null)
let root: am5.Root | null = null

// کشورهایی که باید زرد طلایی باشن
const highlightedCountries = ['IRN', 'TUR', 'IRQ'] // ایران، ترکیه، عراق

onMounted(() => {
  if (!chartdiv.value) return

  root = am5.Root.new(chartdiv.value)

  // چارت نقشه سه‌بعدی
  const chart = root.container.children.push(
    am5map.MapChart.new(root, {
      projection: am5map.geoOrthographic(),
      panX: 'rotateX',
      panY: 'rotateY',
      wheelY: 'zoom',
      homeGeoPoint: { latitude: -15, longitude: 48 },
    })
  )

  // اقیانوس: آبی فضایی تیره + گرادیان حرفه‌ای
  chart.chartContainer.background?.setAll({
    fillOpacity: 1,
  })

  const gradient = am5.RadialGradient.new(root, {
    stops: [
      { color: am5.color('#0b132b') },        // آبی خیلی تیره (مرکز)
      { color: am5.color('#1c2a5e'), opacity: 0.9 },
      { color: am5.color('#2a4a8e'), opacity: 0.6 },
      { color: am5.color('#3a5a9e'), opacity: 0.3 },
    ]
  })
  chart.chartContainer.background?.set('fillGradient', gradient)

  // سری کشورها
  const polygonSeries = chart.series.push(
    am5map.MapPolygonSeries.new(root, {
      geoJSON: am5geodata_worldLow,
    })
  )

  const polygonTemplate = polygonSeries.mapPolygons.template
  polygonTemplate.setAll({
    tooltipText: '{name}',
    interactive: true,
    fill: am5.color('#5a8cc2'),       // آبی روشن برای بقیه کشورها
    stroke: am5.color('#ffffff'),
    strokeWidth: 0.7,
    strokeOpacity: 0.5,
  })

  // حالت hover
  polygonTemplate.states.create('hover', {}).setAll({
    fill: am5.color('#3a6ba5'),
  })

  // ایران، ترکیه و عراق → زرد طلایی
  polygonSeries.mapPolygons.template.adapters.add('fill', (fill, target) => {
    if (target?.dataItem?.dataContext?.id && highlightedCountries.includes(target.dataItem.dataContext.id)) {
      return am5.color('#ffdd00') // زرد طلایی درخشان
    }
    return fill
  })

  // hover روی کشورهای زرد → کمی تیره‌تر
  polygonSeries.mapPolygons.template.adapters.add('fill', (fill, target) => {
    if (target?.dataItem?.dataContext?.id && highlightedCountries.includes(target.dataItem.dataContext.id)) {
      const hoverState = target.states.getKey('hover')
      if (hoverState) hoverState.set('fill', am5.color('#e6c200'))
    }
    return fill
  })

  // خطوط شبکه (گراتیکیول)
  const graticuleSeries = chart.series.push(am5map.GraticuleSeries.new(root, {}))
  graticuleSeries.mapLines.template.setAll({
    stroke: am5.color('#ffffff'),
    strokeOpacity: 0.07,
  })

  // چرخش خودکار بعد از 3 ثانیه
  let rotation: am5.Animation | null = null
  setTimeout(() => {
    rotation = chart.animate({
      key: 'rotationLongitude',
      from: 0,
      to: 360,
      duration: 80000,
      loops: Infinity,
    })
  }, 3000)

  // کلیک = توقف / ادامه چرخش
  chart.chartContainer.events.on('click', () => {
    rotation?.isRunning() ? rotation.stop() : rotation?.play()
  })

  // زوم اولیه روی خاورمیانه (ایران در مرکز)
  chart.zoomToGeoPoint({ latitude: 32, longitude: 53 }, 1.7, true, 1200)
})

onUnmounted(() => {
  root?.dispose()
})
</script>

<style scoped>
.world-map-wrapper {
  width: 100%;
  height: 720px;
  background: #f1f2f2;
  padding: 40px 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
}

.chartdiv {
  width: 100%;
  height: 100%;
  border-radius: 28px;
  overflow: hidden;
  box-shadow: 0 25px 60px rgba(0, 0, 0, 0.35);
}
</style>