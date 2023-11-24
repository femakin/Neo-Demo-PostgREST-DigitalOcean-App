<!-- eslint-disable no-undef -->
<script setup>
import { ref, onMounted } from "vue";

const projects = ref([]);
const loading = ref(false);
const newProject = ref({
  name: "",
  description: "",
  status: "Planned",
  start_date: "",
  end_date: "",
});
const selectedProjectId = ref(null);
const showUpdateModal = ref(false);

onMounted(() => {
  fetchProjects();
});

const fetchProjects = () => {
  loading.value = true;
  fetch(`${process.env.VUE_APP_API_BASE}/projects`)
    .then((response) => response.json())
    .then((data) => {
      projects.value = data;
      loading.value = false;
    })
    .catch((error) => {
      console.error("Error fetching projects:", error);
      loading.value = false;
    });
};

const handleAddProject = (e) => {
  loading.value = true;
  e.preventDefault();
  // console.log(newProject.value, 'dd')
  fetch(`${process.env.VUE_APP_API_BASE}/projects`, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(newProject.value),
  })
    .then(async (response) => {
      if (response.ok) {
        fetchProjects();
        loading.value = false;

        newProject.value = {
          name: "",
          description: "",
          status: "Planned",
          start_date: "",
          end_date: "",
        };
      }

      if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
      }

      newProject.value = {
        name: "",
        description: "",
        status: "Planned",
        start_date: "",
        end_date: "",
      };
    })
    .catch((error) => {
      console.error("Error creating project:", error);
    });
};

const handleUpdateProject = (projectId, updatedValues) => {
  console.log(projectId, "dddd");
  fetch(`${process.env.VUE_APP_API_BASE}/projects?id=eq.${projectId}`, {
    method: "PATCH",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(updatedValues),
  })
    .then((response) => {
      if (response.ok) {
        fetchProjects();
        loading.value = false;

        newProject.value = {
          name: "",
          description: "",
          status: "Planned",
          start_date: "",
          end_date: "",
        };
        showUpdateModal.value = false;
      }

      if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
      }
      newProject.value = {
        name: "",
        description: "",
        status: "Planned",
        start_date: "",
        end_date: "",
      };
    })

    .catch((error) => {
      console.error("Error updating project:", error);
      loading.value = false;
    });
};

const handleShowUpdateModal = (projectId) => {
  const selectedProject = projects.value.find(
    (project) => project.id === projectId,
  );

  if (selectedProject) {
    selectedProjectId.value = projectId;
    newProject.value = { ...selectedProject };
    showUpdateModal.value = true;
  }
};

const handleCloseUpdateModal = () => {
  showUpdateModal.value = false;
};

const handleDeleteProject = (projectId) => {
  loading.value = true;
  fetch(`${process.env.VUE_APP_API_BASE}/projects?id=eq.${projectId}`, {
    method: "DELETE",
  })
    .then((response) => {
      if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
      }

      if (response.ok) {
        fetchProjects();
        loading.value = false;

        newProject.value = {
          name: "",
          description: "",
          status: "Planned",
          start_date: "",
          end_date: "",
        };
        showUpdateModal.value = false;
      }
    })
    .catch((error) => {
      console.error("Error deleting project:", error);
      loading.value = true;
    });
};
</script>

<template>
  <div className="flex min-h-screen flex-col items-center justify-between p-24">
    <div className="text-4xl font-bold mb-8">Project Management App</div>

    <div className="">
      <div v-if="loading">
        <p>Fetching data...</p>
      </div>
      <div v-else className="flex gap-12 flex-wrap ">
        <div v-for="project in projects" :key="project.id" className="border rounded p-4 shadow-md">
          <h2 className="text-lg font-semibold mb-2">{{ project.name }}</h2>
          <p className="text-gray-600">{{ project.description }}</p>
          <p className="text-sm mt-2">Status: {{ project.status }}</p>
          <p className="text-sm">Start Date: {{ project.start_date }}</p>
          <p className="text-sm">End Date: {{ project.end_date }}</p>
          <p className="text-sm">ID: {{ project.id }}</p>

          <button @click="handleDeleteProject(project.id)"
            className="bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600">
            Delete
          </button>

          <button @click="handleShowUpdateModal(project.id)"
            className="bg-yellow-500 text-white px-4 py-2 rounded hover:bg-yellow-600">
            Update
          </button>
        </div>
      </div>
    </div>

    <div className="mt-8">
      <h2 className="text-2xl font-bold mb-4">Add New Project</h2>
      <form className="flex flex-wrap gap-12" @submit.prevent="handleAddProject">
        <div className="flex flex-col mb-4 w-full sm:w-1/2 md:w-1/3">
          <label htmlFor="name" className="block text-sm font-medium text-gray-700">
            Name
          </label>
          <input type="text" id="name" name="name" v-model="newProject.name" class="mt-1 p-2 border rounded w-full"
            required />
        </div>

        <div className="flex flex-col mb-4 w-full sm:w-1/2 md:w-1/3">
          <label htmlFor="description" className="block text-sm font-medium text-gray-700">
            Description
          </label>
          <textarea id="description" name="description" v-model="newProject.description"
            class="mt-1 p-2 border rounded w-full" required></textarea>
        </div>

        <div className="flex flex-col mb-4 w-full sm:w-1/2 md:w-1/3">
          <label htmlFor="status" className="block text-sm font-medium text-gray-700">
            Status
          </label>
          <select id="status" name="status" v-model="newProject.status" class="mt-1 p-2 border rounded w-full" required>
            <option value="Planned">Planned</option>
            <option value="In Progress">In Progress</option>
            <option value="Completed">Completed</option>
          </select>
        </div>

        <div className="flex flex-col mb-4 w-full sm:w-1/2 md:w-1/3">
          <label htmlFor="start_date" className="block text-sm font-medium text-gray-700">
            Start Date
          </label>
          <input type="date" id="start_date" name="start_date" v-model="newProject.start_date"
            class="mt-1 p-2 border rounded w-full" required />
        </div>

        <div className="flex flex-col mb-4 w-full sm:w-1/2 md:w-1/3">
          <label htmlFor="end_date" className="block text-sm font-medium text-gray-700">
            End Date
          </label>
          <input type="date" id="end_date" name="end_date" v-model="newProject.end_date"
            class="mt-1 p-2 border rounded w-full" required />
        </div>

        <div className="w-full">
          <button type="submit" :disabled="loading" className="bg-blue-500 text-white px-4 py-2 rounded" v-if="!loading">
            Submit
          </button>
          <button type="button" :disabled="loading"
            className="bg-blue-500 text-white px-4 py-2 rounded cursor-not-allowed opacity-50" v-else>
            Loading...
          </button>
        </div>
      </form>
    </div>

    <div v-if="showUpdateModal">
      <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center">
        <div className="bg-white p-4 md:p-5 border rounded">
          <h3 className="text-xl font-semibold mb-4">Update Project</h3>
          <form @submit.prevent="handleUpdateProject(selectedProjectId, newProject)">
            <div class="flex flex-col mb-4 w-full">
              <label for="name" class="block text-sm font-medium text-gray-700">Name</label>
              <input type="text" id="name" name="name" v-model="newProject.name" class="mt-1 p-2 border rounded w-full"
                required />
            </div>

            <div class="flex flex-col mb-4 w-full">
              <label for="status" class="block text-sm font-medium text-gray-700">Status</label>
              <select id="status" name="status" v-model="newProject.status" class="mt-1 p-2 border rounded w-full"
                required>
                <option value="Planned">Planned</option>
                <option value="In Progress">In Progress</option>
                <option value="Completed">Completed</option>
              </select>
            </div>

            <button type="button" @click="handleCloseUpdateModal"
              className="bg-red-500 text-white px-4 py-2 rounded mr-2 hover:bg-red-600">
              Cancel
            </button>
            <button type="submit" className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">
              Update
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
