<template>
  <Teleport to="body">
    <base-dialog
      v-if="invalidInput"
      title="Invalid Input"
      @close="confirmError"
    >
      <template #default>
        <p>Unfortunately, at least some inputs are invalid</p>
        <p>Please check all your inputs again.</p>
      </template>
      <template #action>
        <base-button @click="confirmError">Okay</base-button>
      </template>
    </base-dialog>
  </Teleport>
  <base-card>
    <form @submit.prevent="submitForm">
      <div class="form-control">
        <label for="title">Title</label>
        <input type="text" id="title" v-model="title" />
      </div>
      <div class="form-control">
        <label for="description">Description</label>
        <textarea id="description" rows="5" v-model="description"></textarea>
      </div>
      <div class="form-control">
        <label for="link">Link</label>
        <input type="url" id="link" v-model="link" />
      </div>
      <div>
        <base-button type="submit">Submit</base-button>
      </div>
    </form>
  </base-card>
</template>

<script>
export default {
  data() {
    return {
      title: '',
      description: '',
      link: '',
      invalidInput: false,
    };
  },
  inject: ['addResource'],
  methods: {
    submitForm() {
      if (
        this.title.trim().length === 0 ||
        this.description.trim().length === 0 ||
        this.link.trim().length === 0
      ) {
        this.invalidInput = true;
        return;
      }
      const resourceData = {
        id: new Date().toISOString(),
        title: this.title,
        description: this.description,
        link: this.link,
      };
      this.addResource(resourceData);
      this.title = '';
      this.description = '';
      this.link = '';
    },
    confirmError() {
      this.invalidInput = false;
    },
  },
};
</script>

<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  padding: 0.15rem;
  border: 1px solid #ccc;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #3a0061;
  background-color: #f7ebff;
}

.form-control {
  margin: 1rem 0;
}
</style>
