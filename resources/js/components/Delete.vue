<template>
  <button
    type="button"
    :class="myclass"
    data-bs-toggle="modal"
    :data-bs-target="`#${modal}`"
  >
    Delete Survey
  </button>

  <div
    class="modal fade text-start"
    :id="modal"
    tabindex="-1"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel"><b>Survey</b></h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          Are you sure you want to delete the survey?
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-danger" data-bs-dismiss="modal">
            Cancel
          </button>
          <button class="btn btn-primary" @click="deleteSurvey">Confirm</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Delete",
  props: {
    myclass: String,
    surveyID: Number,
    modal: String,
    routeNum: Number,
  },
  data() {
    return {};
  },
  methods: {
    deleteSurvey() {
      axios
        .post("delete/" + this.surveyID)
        .then((response) => {
          $(`#${this.modal}`).modal("hide");
          if (this.routeNum == -1) this.$router.go(-1);
          else if (this.routeNum == 0) this.$router.go(0);
          this.$toast.success(`Survey Successfully Deleted`, {
            position: "top",
            queue: true,
          });
        })
        .catch((errors) => {
          console.log(errors);
        });
    },
  },
};
</script>

<style scoped>
.modal-body {
  font-size: 17px;
}
</style>
