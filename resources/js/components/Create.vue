<template>
  <button
    type="button"
    :class="myclass"
    data-bs-toggle="modal"
    data-bs-target="#create"
  >
    Create Survey
  </button>

  <form
    @submit="create"
    class="modal fade text-start"
    id="create"
    tabindex="-1"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">
            <b>Create Survey</b>
          </h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          <div class="mb-3">
            <label for="survey-name" class="col-form-label">Survey Name</label>
            <input
              type="text"
              class="form-control"
              v-model="formData.name"
              required
            />
          </div>
          <label for="survey-questions" class="col-form-label"
            >Survey Questions</label
          >
          <div
            class="row g-2 mb-2"
            v-for="(question, k) in formData.questions"
            :key="k"
          >
            <div class="col">
              <input
                type="text"
                class="form-control"
                v-model="question.text"
                required
              />
            </div>
            <div class="col-auto">
              <span
                class="btn btn-success btn-sm me-2 mt-1"
                @click="add(k)"
                v-show="k == formData.questions.length - 1"
              >
                Add
              </span>
              <span
                class="btn btn-danger btn-sm mt-1"
                @click="remove(k)"
                v-show="k || (!k && formData.questions.length > 1)"
              >
                Remove
              </span>
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <button type="submit" class="btn btn-primary">Create</button>
        </div>
      </div>
    </div>
  </form>
</template>

<script>
export default {
  name: "Create",
  props: {
    myclass: String,
  },
  data() {
    return {
      formData: {
        id: Number,
        name: "",
        questions: [
          {
            text: "",
          },
        ],
      },
    };
  },
  methods: {
    add(index) {
      this.formData.questions.push({ text: "" });
    },
    remove(index) {
      this.formData.questions.splice(index, 1);
    },
    create() {
      this.formData.id = localStorage.getItem("id");
      axios
        .post("create", this.formData)
        .then((response) => {
          this.formData.name = "";
          this.formData.questions.length = 1;
          this.formData.questions[0].text = "";
          $("#create").modal("hide");
          this.$router.go(0);
          this.$toast.success(`Survey Successfully Created`, {
            position: "top",
            queue: true,
          });
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
