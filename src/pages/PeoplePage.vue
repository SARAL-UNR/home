<template>
  <v-container max-width="900" class="py-12">

    <!-- Page Header -->
    <div class="mb-12">
      <h1 class="page-title mb-2">People</h1>
      <p class="text-medium-emphasis" style="font-size: 1.05rem;">
        The researchers and students behind SARAL. Click on a person's name to learn more about them.
      </p>
    </div>

    <!-- Group Sections -->
    <div v-for="group in groups" :key="group.description" class="mb-12">

      <!-- Group Heading -->
      <div class="group-heading d-flex align-center mb-6">
        <span class="group-label mr-4">{{ group.description }}</span>
        <v-divider />
      </div>

      <!-- People Grid -->
      <v-row>
        <v-col
          v-for="person in group.members"
          :key="person.id"
          cols="6"
          sm="3"
          md="2"
        >
        <div
          class="person-card"
          :class="{ 'person-card--linked': person.slug }"
          @click="handleClick(person)"
        >
            <!-- Portrait -->
            <div class="portrait-wrapper mb-3">
              <img
                v-if="person.photo"
                :src="person.photo"
                :alt="person.name"
                class="portrait-img"
              />
              <div v-else class="portrait-fallback d-flex align-center justify-center">
                <span class="portrait-initials">{{ initials(person.name) }}</span>
              </div>
            </div>

            <div class="person-name">{{ person.name }}</div>
            <div class="person-years text-medium-emphasis">{{ person.years }}</div>
            <div class="person-description text-medium-emphasis">{{ person.description }}</div>
          </div>
        </v-col>
      </v-row>

    </div>

  </v-container>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'
const router = useRouter()

const people = ref([
  // Lab Director
  {
    id: 1,
    slug: 'https://www.parikshitmaini.com/',
    name: 'Dr. Parikshit Maini',
    description: 'Principal Investigator',
    group: 'Lab Director',
    years: '2023 - Present',
    photo: `${import.meta.env.BASE_URL}images/people/parikshit-maini.png`,
  },

  // PhD Students
  {
    id: 2,
    slug: 'https://arifahmed.net/',
    name: 'Arif Ahmed',
    description: 'PhD Student',
    group: 'PhD Students',
    years: '2023 - Present',
    photo: `${import.meta.env.BASE_URL}images/people/arif-ahmed.png`,
  },
  {
    id: 3,
    slug: 'https://www.linkedin.com/in/nathaniel-rose-64916b165/',
    name: 'Nathaniel Rose',
    description: 'PhD Student',
    group: 'PhD Students',
    years: '2023 - Present',
    photo: `${import.meta.env.BASE_URL}images/people/nathaniel-rose.png`,
  },

  // Master's Students
  {
    id: 4,
    slug: 'https://www.linkedin.com/in/hannah-chuang-0a42981ba/',
    name: 'Hannah Chuang',
    description: "Master's Student",
    group: "Master's Students",
    years: '2024 - 2026',
    photo: `${import.meta.env.BASE_URL}images/people/hannah-chuang.png`,
  },
  {
    id: 5,
    slug: 'https://www.linkedin.com/in/emanuel-gutierrez-b91352250/',
    name: 'Emanuel Gutierrez-Cornejo',
    description: "Master's Student",
    group: "Master's Students",
    years: '2024 - Present',
    photo: `${import.meta.env.BASE_URL}images/people/emanuel-gutierrez-cornejo.png`,
  },
  {
    id: 6,
    slug: 'https://www.linkedin.com/in/jairo-cadena-mendez/',
    name: 'Jairo Cadena-Mendez',
    description: "Master's Student",
    group: "Master's Students",
    years: '2026 - Present',
    photo: `${import.meta.env.BASE_URL}images/people/jairo-cadena-mendez.jpg`,
  },
  {
    id: 7,
    slug: 'https://www.linkedin.com/in/dominic-palmieri-a2b249235',
    name: 'Dominic Palmieri',
    description: "Master's Student",
    group: "Master's Students",
    years: '2026 - Present',
    photo: `${import.meta.env.BASE_URL}images/people/dominic-palmieri.png`,
  },


  // Undergraduates
  {
    id: 8,
    slug: 'https://www.linkedin.com/in/yovan-hirales/',
    name: 'Yovan Hirales',
    description: 'Undergraduate',
    group: 'Undergraduates',
    years: '2026 - Present',
    photo: `${import.meta.env.BASE_URL}images/people/yovan-hirales.png`,
  }, 

  // Interns
  {
    id: 13,
    slug: 'https://www.linkedin.com/in/vansh-sachdeva-b367a033b/',
    name: 'Vansh Sachdeva',
    description: "Former Members",
    group: "Former Members",
    years: '2026 - Present',
    photo: `${import.meta.env.BASE_URL}images/people/vansh-sachdeva.jpg`,
  },

  // Alumni — no slug needed, no individual page
  /*{
    id: 9,
    slug: '',
    name: 'David Park',
    description: 'PhD Alumni → Google DeepMind',
    group: 'Master\'s Alumni',
    years: '',
    photo: `${import.meta.env.BASE_URL}images/people/david-park.png`,
  },
  {
    id: 10,
    slug: '',
    name: 'Priya Nair',
    description: 'MS Alumni → Boston Dynamics',
    group: 'Phd Alumni',
    years: '',
    photo: `${import.meta.env.BASE_URL}images/people/priya-nair.png`,
  },
  {
    id: 11,
    slug: '',
    name: 'John Doe',
    description: 'Bachelor\'s Alumni → XYZ Company',
    group: 'Bachelor\'s Alumni',
    years: '',
    photo: `${import.meta.env.BASE_URL}images/people/john-doe.png`,
  },
  {
    id: 12,
    slug: '',
    name: 'Jane Smith',
    description: 'Former REU Student',
    group: 'Former REU Students',
    years: '',
    photo: `${import.meta.env.BASE_URL}images/people/jane-smith.png`,
  } */
])

const groupOrder = ["Lab Director", "PhD Students", "Master's Students", "Undergraduates", "Interns", "Former Members", "Master's Alumni", "Bachelor's Alumni", "Previous Interns", "Former REU Students"]

const groups = computed(() =>
  groupOrder
    .map(description => ({
      description,
      members: people.value.filter(p => p.group === description),
    }))
    .filter(g => g.members.length > 0)
)

function initials(name) {
  return name.split(' ').filter(Boolean).map(n => n[0]).slice(0, 2).join('').toUpperCase()
}

function handleClick(person) {
  if (!person.slug) return
  if (person.slug.startsWith('http')) {
    window.open(person.slug, '_blank')
  } else {
    router.push({ name: 'people-individual', params: { slug: person.slug } })
  }
}
</script>

<style scoped>
.page-title {
  font-size: clamp(2rem, 5vw, 3rem);
  font-weight: 800;
  letter-spacing: -1.5px;
  line-height: 1.1;
}

.group-label {
  font-size: 1.1rem;
  font-weight: 700;
  letter-spacing: -0.5px;
  white-space: nowrap;
  color: rgb(var(--v-theme-primary));
}

.person-card {
  display: flex;
  flex-direction: column;
}



/* Show pointer and hover effect only for people with a slug */
.person-card--linked {
  cursor: pointer;
}

.person-card--linked .portrait-wrapper:hover {
  border-color: rgba(var(--v-theme-primary), 1);
  transform: translateY(-3px);
}

.portrait-wrapper {
  width: 100%;
  aspect-ratio: 3 / 4;
  overflow: hidden;
  border-radius: 8px;
  border: 2px solid rgba(var(--v-theme-primary), 0.12);
  transition: border-color 0.2s ease, transform 0.2s ease;
  background-color: rgba(0, 0, 0, 0.05);
}

.portrait-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: top center;
  display: block;
}

.portrait-fallback {
  width: 100%;
  height: 100%;
  background-color: rgb(var(--v-theme-primary));
}

.portrait-initials {
  font-size: 2rem;
  font-weight: 700;
  color: white;
}

.person-name {
  font-size: 0.9rem;
  font-weight: 600;
  line-height: 1.3;
  margin-bottom: 2px;
}

.person-years {
  font-size: 0.775rem;
  line-height: 1.4;
}

.person-description {
  font-size: 0.775rem;
  line-height: 1.4;
}
</style>