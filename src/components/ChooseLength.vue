<template>
  <div>
    <b-jumbotron>
      <b-form @submit="onSubmit" @reset="onReset" v-if="show">
        <b-form-group
          id="input-group-2"
          label="Quiz Length:"
          label-for="input-2"
        >
          <b-form-input
            id="input-2"
            type="number"
            v-model="form.number"
            placeholder="Enter Your Desired Quiz Length"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group>
          <b-form-select



          v-model="form.category"
      :options="categories"
      class="mb-3"
      value-field="id"
      text-field="name"
      disabled-field="notEnabled"
      >
          </b-form-select>
        </b-form-group>

        <b-form-group>
          <b-form-select
            v-model="form.type"
            :options="['', 'multiple', 'boolean']"
          ></b-form-select>
        </b-form-group>

                <b-form-group>
          <b-form-select
            v-model="form.difficulty"
            :options="['', 'easy', 'medium','hard']"
          ></b-form-select>
        </b-form-group>


        <b-button type="submit" variant="primary" class="mr-2">Submit</b-button>
        <b-button type="reset" variant="danger">Reset</b-button>
      </b-form>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    categories: Array,
    fetchRequest: Function,
  },
  data() {
    return {
      form: {
        number: 1,
        category: 0,
        type: "",
        difficulty: "",
      },
      show: true,
    };
  },
  methods: {
    onSubmit(event) {
      event.preventDefault();
      this.fetchRequest({...this.form});

    },
    onReset(event) {
      event.preventDefault();
      this.form.number = 0;
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
  },
};
</script>
