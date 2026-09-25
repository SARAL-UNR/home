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

  <div v-for="(vid, i) in section.videos" :key="i" class="mb-4">
    <!-- YouTube embed -->
    <iframe
      v-if="vid.type === 'youtube'"
      :src="vid.src"
      style="width: 100%; aspect-ratio: 16/9; border-radius: 10px; border: none;"
      allowfullscreen
    />
    <!-- Local video -->
    <video
      v-else
      :src="vid.src"
      controls
      style="width: 100%; border-radius: 10px;"
    />
    <p v-if="vid.caption" class="video-caption mt-2">{{ vid.caption }}</p>
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
      <!--
      <div class="d-flex align-center mb-3">
        <span class="section-label mr-4">Publications, Datasets and Other Materials</span>
        <v-divider />
      </div>
    -->
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
  
      <!-- Empty state
      <p v-if="!project.publications || project.publications.length === 0" class="text-medium-emphasis" style="font-size: 0.875rem;">
        No publications, datasets, or other materials are associated with this project yet.
      </p>
    -->
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
      conference: 'ICRA 2027',
      videos: [
        {
          src: `${import.meta.env.BASE_URL}videos/RSMS_ICRA2027_1.mp4`,
          //caption: 'MoistureMapper navigating a farm plot.',
          type: 'local'
        },
        {
          src: `${import.meta.env.BASE_URL}videos/RSMS_ICRA2027_2.mp4`,
          //caption: 'MoistureMapper navigating a field.',
          type: 'local'
        },
      ],
      images: [],
      publications: [],
    },
    {
      conference: 'IROS 2026',
      videos: [
        {
          src: `https://www.youtube.com/embed/eHAamgfhVQw`,
          //caption: 'MoistureMapper navigating a farm plot.',
          type: 'youtube'
        }
      ],
      images: [],
      publications: [],
    },
    {
      conference: 'CASE 2025',
      videos: [
        {
          // Youtube link
          src: `https://www.youtube.com/embed/S4bJ4tRzObg`,
          //caption: 'MoistureMapper navigating a farm plot.',
          type: 'youtube'
        },
      ],
      images: [],
      publications: [
        {
          title: 'MoistureMapper: An Autonomous Mobile Robot for High-Resolution Soil Moisture Mapping at Scale',
          authors: 'Nathaniel Rose, Hannah Chuang, Manuel A Andrade-Rodriguez, Rishi Parashar, Dani Or, Parikshit Maini',
          venue: '2025 IEEE 21st International Conference on Automation Science and Engineering (CASE)',
          type: 'conference',
          review: false,
          url: 'https://ieeexplore.ieee.org/document/11163809',
          },
        ],
      },
    ],
  
  },

  {
    slug: 'stability-aware-navigation',
    title: 'Stability Aware Navigation',
    description: `During off-road navigation wheeled mobile robots experience terrain-induced disturbances that can degrade on-board task performance. 
    To account for traversal roughness and to design roughness-aware navigation controllers, quantification of robot-terrain interaction becomes imperative. 
    Existing measures of traversal roughness are often derived directly from proprioceptive signals. These IMU-derived metrics are widely used as supervisory 
    signals in learning based navigation controllers. However, it is hard to independently evaluate the robustness of these metrics to capture the 
    disturbances experienced by the robot and the effect of signal noise. In this work, we introduce the Count-Circles-Crossed (C3) score,
    an interpretable vision-based metric that quantifies terrain-induced platform disturbance by measuring the image frame displacement of a static reference 
    over a time window. C3 provides an exteroceptive measure of platform disturbance that is independent of proprioceptive measurements. 
    We compare and contrast C3 with commonly used IMU-derived roughness measures and show that it captures traversal roughness across different terrains 
    and speeds. We then use C3 as a supervisory signal to train a LIMU-BERT architecture based prediction model to learn a mapping from onboard IMU and speed 
    measurements to traversal roughness (C3). This allows us to still use proprioceptive sensing for online roughness estimation validated using independent 
    sensor measurements while removing the need for image processing during deployment.`,

    images: [
      { src: `${import.meta.env.BASE_URL}images/projects/C31.png`, caption: '' },
      { src: `${import.meta.env.BASE_URL}images/projects/C32.jpeg`, caption: '' },
      { src: `${import.meta.env.BASE_URL}images/projects/C33.png`, caption: '' },
      { src: `${import.meta.env.BASE_URL}images/projects/C34.png`, caption: '', rotation: 90 },
      { src: `${import.meta.env.BASE_URL}images/projects/C35.png`, caption: ''},
    ],
      media: [
        {
          conference: 'General',
          videos: [],
          images: [
            { src: `${import.meta.env.BASE_URL}images/projects/C31.png`, caption: '' },
            { src: `${import.meta.env.BASE_URL}images/projects/C32.jpeg`, caption: '' },
            { src: `${import.meta.env.BASE_URL}images/projects/C33.png`, caption: '' },
            { src: `${import.meta.env.BASE_URL}images/projects/C34.png`, caption: '', rotation: 90 },
            { src: `${import.meta.env.BASE_URL}images/projects/C35.png`, caption: ''},
          ],
        publications: [
          ],
        },
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
    description: `This project consists of an autonomous drone that precisely applies chemicals to sorghum whorls, which are developing leaves fr
    om the plant's main stalk, to improve selective plant breeding processes. This is achieved through four subsystems: perception, 
    embedded systems, visual servoing, and controls. The drone is able to fly missions autonomously, recognize the whorl through its AI model, 
    maneuver itself over the plant, apply the chemical, and continue its progress throughout an entire field. This has widespread global implications, 
    as it makes selective breeding easier, enabling plants to be more drought-resistant and increasing crop yields. It also removes the need for humans 
    to manually apply these chemicals, reducing labor costs and exposure to dangerous chemicals.`,
    highlights: ['Precision spraying', 'Plant breeding', 'Agronomy'],
    images: [
      { src: `${import.meta.env.BASE_URL}images/projects/Spray1.png`, caption: '' },
    ],
    publications: [],
    media: [
        {
          conference: 'General',
          videos: [
            {
              src: `${import.meta.env.BASE_URL}videos/Spray_Gen_1.mp4`,
              //caption: 'MoistureMapper navigating a farm plot.',
              type: 'local'
            },
            {
              src: `${import.meta.env.BASE_URL}videos/Spray_Gen_2.mp4`,
              //caption: 'MoistureMapper navigating a field.',
              type: 'local'
            },
          ],
          images: [
          ],
          }
        ]
  },
  {
    slug: 'ground-air-robotics',
    title: 'Ground-Air Robotics',
    people: 'Jairo Cadena-Mendez',
    date: 'DATES HERE',
    description: `The Ground-Air Robotics Collaboration is a cooperative autonomous system designed to extend the operational range of a UAV 
    by using a UGV as a mobile refueling station where both vehicles coordinate to meet at feasible rendezvous points. The objective is to develop an 
    online replanner that dynamically adapts to stochastic variables the UAV may encounter while visiting its targets.`,
    highlights: ['Ground-air robotics', 'Multi-robot systems', 'Robotic coordination'],

    media: [
      {
        conference: 'General',
        videos: [
          {
            src: `${import.meta.env.BASE_URL}videos/GAC_Gen_1.mp4`,
            //caption: 'MoistureMapper navigating a farm plot.',
            type: 'local'
          },
        ],
        images: [
        ],
        publications: [],
      }
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
  text-align: center;
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