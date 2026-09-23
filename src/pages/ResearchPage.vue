<template>
  <v-container max-width="900" class="py-12">

    <!-- Page Header -->
    <div class="mb-10">
      <h1 class="page-title mb-2">Research</h1>
    </div>

    <!-- Groups -->
    <div v-for="group in projectsByValue" :key="group.value" class="mb-10">

      <!-- Group Heading -->
      <div class="d-flex align-center mb-4">
        <span class="year-label mr-4">{{ group.value }}</span>
        <v-divider />
      </div>

      <!-- Project Cards -->
      <div class="projects-grid">
        <v-card
          v-for="project in group.projects"
          :key="project.id"
          flat
          border
          class="project-card pa-5"
          :class="{ 'project-card--linked': project.slug }"
          :ripple="false"
          @click="handleClick(project)"
        >
          <!-- Title + arrow -->
          <div class="d-flex align-center justify-space-between mb-1">
            <div class="project-title">{{ project.title }}</div>
            <v-icon v-if="project.slug" size="18" color="primary" class="ml-2">mdi-arrow-right</v-icon>
          </div>

          <!-- Preview image -->
          <div v-if="project.images && project.images.length > 0">
            <v-img :src="project.images[0].src" rounded="lg" />
          </div>
        </v-card>
      </div>
    </div>
  </v-container>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const projects = ref([
  ///{
    ///id: 1,
    ///slug: 'robohydra',
    ///value: 'Current Research',
    ///title: 'RoboHydra',
    ///images: [
      ///{
        //src: `${import.meta.env.BASE_URL}images/projects/Hydra2.png`,
        ///caption: 'RoboHydra',
      ///},
    ///],
  ///},
  {
    id: 3,
    slug: 'rsms',
    value: 'Current Research',
    title: 'MoistureMapper: Robotic Soil Moisture Sensing',
    images: [
      {
        src: `${import.meta.env.BASE_URL}images/projects/RSMS1.png`,
        caption: '',
      },
    ],
  },
  {
    id: 4,
    slug: 'stability-aware-navigation',
    value: 'Current Research',
    title: 'Stability Aware Navigation',
    images: [
      { src: `${import.meta.env.BASE_URL}images/projects/C32.jpeg`, caption: '' },
    ],
  },
  {
    id: 5,
    slug: 'plant-phenotyping-lai',
    value: 'Current Research',
    title: 'Plant Phenotyping: Leaf Area Index (LAI)',
    images: [
      { src: `${import.meta.env.BASE_URL}images/projects/LAI1.png`, caption: '' },
    ],
  },
  {
    id: 6,
    slug: 'robotics-ai-plant-breeding',
    value: 'Current Research',
    title: 'Autonomous Drone Sprayer',
    images: [
      { src: `${import.meta.env.BASE_URL}images/projects/Spray1.png`, caption: '' },
    ],
  },
  {
    id: 2,
    slug: 'ground-air-robotics',
    value: 'Current Research',
    title: 'Ground-Air Robotics',
    images: [
      { src: `${import.meta.env.BASE_URL}images/projects/GAC2.png`, caption: 'Caption for image 2a' },
    ],
  },
])

const projectsByValue = computed(() => {
  const values = [...new Set(projects.value.map(p => p.value))].sort((a, b) => b - a)
  return values.map(value => ({
    value,
    projects: projects.value.filter(p => p.value === value),
  }))
})

function handleClick(project) {
  if (!project.slug) return
  router.push({ name: 'research-individual', params: { slug: project.slug } })
}

// Show a short preview on the list card
function truncate(text, max = 40) {
  const trimmed = text.trim()
  return trimmed.length > max ? trimmed.slice(0, max).trimEnd() + '…' : trimmed
}
</script>

<style scoped>
.page-title {
  font-size: clamp(2rem, 5vw, 3rem);
  font-weight: 800;
  letter-spacing: -1.5px;
  line-height: 1.1;
}

.year-label {
  font-size: 1.25rem;
  font-weight: 700;
  letter-spacing: -0.5px;
  white-space: nowrap;
  color: rgb(var(--v-theme-primary));
}

.projects-grid {
  columns: 3;
  column-gap: 16px;
}

.project-card {
  break-inside: avoid;
  margin-bottom: 16px;
  border-radius: 10px !important;
  transition: box-shadow 0.15s ease;
}

.project-card--linked {
  cursor: pointer;
}

.project-card--linked:hover {
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1) !important;
}

.project-title {
  font-size: 0.975rem;
  font-weight: 600;
  line-height: 1.4;
}
</style>