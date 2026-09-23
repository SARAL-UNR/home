<template>
  <v-container max-width="900" class="py-12">

    <div v-if="project">

      <!-- Back button -->
      <v-btn
        variant="text"
        color="primary"
        prepend-icon="mdi-arrow-left"
        :to="{ name: 'research' }"
        class="mb-6 back-btn"
      >
        Back to Research
      </v-btn>

      <!-- Title + meta -->
      <h1 class="project-title mb-2">{{ project.title }}</h1>
      <!--<div class="project-meta text-medium-emphasis mb-6">{{ project.date }}</div> -->

      <!-- Description -->
      <div v-if="project.description" class="mb-8">
        <div class="d-flex align-center mb-3">
          <!-- <span class="section-label mr-4">Overview</span> -->
          <v-divider />
        </div>
        <p class="body-text">{{ project.description }}</p>
      </div>

<!-- Media Sections -->
<div v-for="section in project.media" :key="section.conference" class="mb-8">
  <div class="d-flex align-center mb-3">
    <span class="section-label mr-4">{{ section.conference }}</span>
    <v-divider />
  </div>

  <!-- Video -->
  <div v-if="section.video" class="mb-4">
    <video :src="section.video" controls style="width: 100%; border-radius: 10px;" />
    <p v-if="section.videoCaption" class="video-caption mt-2">{{ section.videoCaption }}</p>
  </div>

  <!-- Images -->
  <v-row v-if="section.images && section.images.length > 0">
    <v-col
      v-for="(img, i) in section.images"
      :key="i"
      :cols="section.images.length === 1 ? 12 : 6"
    >
      <div :style="img.rotation === 90 || img.rotation === 270 ? 'overflow: hidden; height: 300px;' : ''">
        <v-img :src="img.src" rounded="lg" :style="img.rotation ? `transform: rotate(${img.rotation}deg)` : ''" />
      </div>
      <div v-if="img.caption" class="img-caption-below mt-1">{{ img.caption }}</div>
    </v-col>
  </v-row>

  <!-- Publications within this conference section -->
  <div v-if="section.publications && section.publications.length > 0" class="mt-4">
    <v-card
      v-for="pub in section.publications"
      :key="pub.title"
      flat border
      class="pub-card mb-3 pa-4"
    >
      <div class="pub-title mb-1">{{ pub.title }}</div>
      <div class="pub-authors text-medium-emphasis mb-2">{{ pub.authors }}</div>
      <div class="d-flex align-center flex-wrap" style="gap: 4px;">
        <v-chip size="small" color="primary" variant="tonal" label>{{ pub.venue }}</v-chip>
        <v-btn v-if="pub.pdfUrl" :href="pub.pdfUrl" target="_blank" variant="text" size="small" color="primary" prepend-icon="mdi-file-pdf-box" class="link-btn">PDF</v-btn>
        <v-btn v-if="pub.arxivUrl" :href="pub.arxivUrl" target="_blank" variant="text" size="small" color="primary" prepend-icon="mdi-open-in-new" class="link-btn">arXiv</v-btn>
        <v-btn v-if="pub.codeUrl" :href="pub.codeUrl" target="_blank" variant="text" size="small" color="primary" prepend-icon="mdi-github" class="link-btn">Code</v-btn>
      </div>
    </v-card>
  </div>

</div> 

          <!-- Related Publications -->
    <div class="mb-8">
      <div class="d-flex align-center mb-3">
        <span class="section-label mr-4">Publications, Datasets and Other Materials</span>
        <v-divider />
      </div>
      <v-card
        v-for="pub in project.publications"
        :key="pub.title"
        flat border
        class="pub-card mb-3 pa-4"
      >
        <div class="pub-title mb-1">{{ pub.title }}</div>
        <div class="pub-authors text-medium-emphasis mb-2">{{ pub.authors }}</div>
        <div class="d-flex align-center flex-wrap" style="gap: 4px;">
          <v-chip size="small" color="primary" variant="tonal" label>{{ pub.venue }}</v-chip>
          <v-btn v-if="pub.pdfUrl" :href="pub.pdfUrl" target="_blank" variant="text" size="small" color="primary" prepend-icon="mdi-file-pdf-box" class="link-btn">PDF</v-btn>
          <v-btn v-if="pub.arxivUrl" :href="pub.arxivUrl" target="_blank" variant="text" size="small" color="primary" prepend-icon="mdi-open-in-new" class="link-btn">arXiv</v-btn>
          <v-btn v-if="pub.codeUrl" :href="pub.codeUrl" target="_blank" variant="text" size="small" color="primary" prepend-icon="mdi-github" class="link-btn">Code</v-btn>
        </div>
      </v-card>
  
      <!-- Empty state -->
      <p v-if="!project.publications || project.publications.length === 0" class="text-medium-emphasis" style="font-size: 0.875rem;">
        No publications, datasets, or other materials are associated with this project yet.
      </p>
    </div>

</div>

    <!-- Empty state -->
    <p v-else class="text-medium-emphasis" style="font-size: 0.875rem;">
      Project not found.
    </p>

  </v-container>
</template>

<script setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()

// ── Project data ──────────────────────────────────────────────────────────────
// slug must match the slug in ResearchPage.vue
// team[].slug can be an internal person slug or an external URL
const projects = [
  {
    slug: 'robohydra',
    title: 'RoboHydra',
    people: 'Dominic Palmieri, Emanuel Gutierrez-Cornejo',
    date: 'Aug 2026 - Present',
    description: `The RoboHydra is an autonomous ground robot designed for use in sheep grazing environments to premote uniform grazing patterns and monitor flock health`,
    highlights: ['Autonomous fluid transportation', 'Stability prediction', 'Livestock-robot interaction'],
    images: [
      //{ src: `${import.meta.env.BASE_URL}images/projects/Hydra1.png`, caption: 'RoboHydra system overview' },
      //{ src: `${import.meta.env.BASE_URL}images/projects/Hydra2.png`, caption: '' },
    ],
    publications: [],
    team: [
      { name: 'Dominic Palmieri', slug: 'dominic-palmieri' },
      { name: 'Emanuel Gutierrez-Cornejo', slug: 'emanuel-gutierrez-cornejo' },
    ],
  },
  {
  slug: 'rsms',
  title: 'MoistureMapper: Robotic Soil Moisture Sensing',
  description: `The MoistureMapper is an autonomous robot that can measure soil moisture at points of interest of a farm plot. The goal is to build a comprehensive map of the soil moisture across the field.`,
  media: [
    {
      conference: 'IROS 2026',
      video: `${import.meta.env.BASE_URL}videos/RSMSvid1.mp4`,
      videoCaption: 'MoistureMapper navigating a farm plot.',
      images: [],
    },
    {
      conference: 'CASE 2025',
      video: null,
      videoCaption: '',
      images: [
        { src: `${import.meta.env.BASE_URL}images/projects/RSMS1.png`, caption: '' },
        //{ src: `${import.meta.env.BASE_URL}images/projects/RSMS2.png`, caption: '' },
      ],
    },
  ],
  publications: [],
  },

  {
    slug: 'stability-aware-navigation',
    title: 'Stability Aware Navigation',
    people: 'Emanuel Gutierrez-Cornejo, Arif Ahmed, Nathaniel Rose, Dominic Palmieri',
    date: 'June 2024 - Present',
    description: `The goal is to predict a robot stability score between 0 and 1. We do this by training a novel vision-based stability metric using count-circle-crossings (C3) score. The method is a data-based learning network trained on C3 score to learn current stability from IMU and velocity.`,
    highlights: ['Stability prediction', 'IMU sensing', 'Deep learning'],
    images: [
      { src: `${import.meta.env.BASE_URL}images/projects/C31.png`, caption: '' },
      { src: `${import.meta.env.BASE_URL}images/projects/C32.jpeg`, caption: '' },
      { src: `${import.meta.env.BASE_URL}images/projects/C33.png`, caption: '' },
      { src: `${import.meta.env.BASE_URL}images/projects/C34.png`, caption: '', rotation: 90 },
    ],
    publications: [],
    team: [
      { name: 'Emanuel Gutierrez-Cornejo', slug: 'emanuel-gutierrez-cornejo' },
      { name: 'Arif Ahmed', slug: 'arif-ahmed' },
      { name: 'Nathaniel Rose', slug: 'nathaniel-rose' },
      { name: 'Dominic Palmieri', slug: 'dominic-palmieri' },
    ],
  },
  {
    slug: 'plant-phenotyping-lai',
    title: 'Plant Phenotyping: Leaf Area Index (LAI)',
    people: 'Arif Ahmed',
    date: 'Jan 2025 - Present',
    description: `Mobile manipulator and UAV work together to phenotype crops for selecting better yielding irrigation strategies.`,
    highlights: ['Mobile manipulation', 'Precision agriculture', 'Phenotyping'],
    images: [
      { src: `${import.meta.env.BASE_URL}images/projects/LAI1.png`, caption: '' },
      { src: `${import.meta.env.BASE_URL}images/projects/LAI2.png`, caption: '' },
      { src: `${import.meta.env.BASE_URL}images/projects/LAI3.png`, caption: '' },
      { src: `${import.meta.env.BASE_URL}images/projects/LAI4.png`, caption: '' },
      { src: `${import.meta.env.BASE_URL}images/projects/LAI5.png`, caption: '' },
      { src: `${import.meta.env.BASE_URL}images/projects/LAI6.png`, caption: '' },
      { src: `${import.meta.env.BASE_URL}images/projects/LAI7.png`, caption: '' },
      { src: `${import.meta.env.BASE_URL}images/projects/LAI8.png`, caption: '' },
    ],
    publications: [],
    team: [
      { name: 'Arif Ahmed', slug: 'arif-ahmed' },
    ],
  },
  {
    slug: 'robotics-ai-plant-breeding',
    title: 'Autonomous Drone Sprayer',
    people: 'Jairo Cadena-Mendez, Yovan Hirales',
    date: 'Sep 2026 - Present',
    description: `Drone based precision spraying of agrochemicals on a field-wide scale. Work with farmers and breeders to apply treatments to corn, sorghum, and onion crops.`,
    highlights: ['Precision spraying', 'Plant breeding', 'Agronomy'],
    images: [
      { src: `${import.meta.env.BASE_URL}images/projects/Spray1.png`, caption: '' },
    ],
    publications: [],
    team: [
      { name: 'Jairo Cadena-Mendez', slug: 'jairo-cadena-mendez' },
      { name: 'Yovan Hirales', slug: 'yovan-hirales' },
    ],
  },
  {
    slug: 'ground-air-robotics',
    title: 'Ground-Air Robotics',
    people: 'Jairo Cadena-Mendez',
    date: 'DATES HERE',
    description: `Description Here`,
    highlights: ['Ground-air robotics', 'Multi-robot systems', 'Robotic coordination'],
    images: [
      { src: `${import.meta.env.BASE_URL}images/projects/GAC1.png`, caption: '' },
      { src: `${import.meta.env.BASE_URL}images/projects/GAC2.png`, caption: '' },
    ],
    publications: [],
    team: [
      { name: 'Jairo Cadena-Mendez', slug: 'jairo-cadena-mendez' },
    ],
  }
]

const project = computed(() =>
  projects.find(p => p.slug === route.params.slug) ?? null
)
</script>

<style scoped>
.back-btn {
  font-size: 0.875rem;
  text-transform: none;
  letter-spacing: 0;
}

.project-title {
  font-size: clamp(1.5rem, 4vw, 2.25rem);
  font-weight: 800;
  letter-spacing: -1px;
  line-height: 1.1;
}

.project-meta {
  font-size: 0.9rem;
  font-weight: 500;
}

.section-label {
  font-size: 1.1rem;
  font-weight: 700;
  letter-spacing: -0.5px;
  white-space: nowrap;
  color: rgb(var(--v-theme-primary));
}

.body-text {
  font-size: 0.9rem;
  line-height: 1.7;
  white-space: pre-line;
}

.img-caption-below {
  font-size: 0.78rem;
  color: rgba(var(--v-theme-on-surface), 0.55);
  text-align: center;
  line-height: 1.4;
}

.video-caption {
  font-size: 0.825rem;
  line-height: 1.6;
  color: rgba(var(--v-theme-on-surface), 0.7);
}

.pub-card {
  border-radius: 10px !important;
}

.pub-title {
  font-size: 0.9rem;
  font-weight: 600;
  line-height: 1.4;
}

.pub-authors {
  font-size: 0.825rem;
}

.link-btn {
  font-size: 0.8rem !important;
  text-transform: none !important;
  letter-spacing: 0 !important;
}
</style>