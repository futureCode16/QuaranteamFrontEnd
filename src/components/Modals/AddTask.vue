<template>
  <div>
    <v-row justify="center">
      <v-dialog v-model="dialog" persistent max-width="290">
        <v-card max-width="350px">
          <v-toolbar color="orange" dark flat>
            <v-toolbar-title>Add Task</v-toolbar-title>
          </v-toolbar>
          <br>
          <v-card-text>
            <v-form ref="task-form">
              <v-text-field label="Name of task" :prepend-icon="'mdi-sword'" v-model="name_task"/>
              <v-select
                :items="noOfStudents"
                menu-props="auto"
                label="Number of students"
                hide-details
                :prepend-icon="'mdi-pound-box'"
                single-line
                v-model="no_student"
              ></v-select>
              <br>
              <v-select
                :items="difficulty"
                menu-props="auto"
                label="Difficulty"
                hide-details
                :prepend-icon="'mdi-gavel'"
                single-line
                v-model="task_difficulty"
              ></v-select>
              <br>
              <v-select
                :items="center"
                menu-props="auto"
                label="Location"
                hide-details
                :prepend-icon="'mdi-map-marker-radius'"
                single-line
                v-model="location"
              ></v-select>
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="error" @click="Cancel">Cancel</v-btn>
            <v-btn color="success" @click="loader = 'loading5'" :loading="loading5" >Done</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <Alert/>
    </v-row>
    <SuccessfullyAdded/>
  </div>
</template>

<script>
import SuccessfullyAdded from "@/components/Snackbars/SuccessfullyAdded.vue";
import Alert from "./Alert.vue";

export default {
  name: "AddTask",
  data() {
    return {
      loading5: false,
      loader: null,
      dialog: false,
      name_task: "",
      no_student: "",
      task_difficulty: "",
      location: "",
      noOfStudents: [
        "1",
        "2",
        "3",
        "4",
        "5",
        "6",
        "7",
        "8",
        "9",
        "10",
        "11",
        "12"
      ],
      difficulty: ["Light", "Hard"],
      center: ["Center 1", "Center 2"]
    };
  },
  watch: {
    loader() {
      const l = this.loader;
      this[l] = !this[l];
      setTimeout(() => (this[l] = false), 3000);

      this.loader = null;
    }
  },
  components: {
    SuccessfullyAdded,
    Alert
  },
  mounted() {
    this.$bus.$on("show-task-form", show => {
      this.dialog = show;
    });
  },
  methods: {
    AddTask() {
      if (
        this.name_task == "" ||
        this.no_student == "" ||
        this.location == "" ||
        this.task_difficulty == ""
      ) {
        this.$bus.$emit("empty-fields", true);
        this.name_task = "";
        this.no_student = "";
        this.location = "";
        this.task_difficulty = "";
        this.dialog = false;
      } else {
        var new_Task = {
          sTasks: this.name_task,
          NoOfStudents: parseInt(this.no_student),
          Center: this.location,
          Difficulty: this.task_difficulty
        };
        setTimeout(() => {
          this.dialog = false;
          this.$store.commit("addTask", new_Task);
          this.$bus.$emit("Successfully-added", {
            task: this.name_task,
            show: true
          });
          (this.name_task = ""),
            (this.no_student = ""),
            (this.location = ""),
            (this.task_difficulty = "");
        }, 2000);
      }
      // this.$store
      //   .dispatch("AddTask", new_Task)
      //   .then(res => {
      //     console.log(res, " res")
      //     this.$store.commit("addTask", res);
      //   })
      //   .catch(err => {
      //     console.log(err);
      //   });
    },
    Cancel() {
      this.name_task = "";
      this.no_student = "";
      this.location = "";
      this.task_difficulty = "";
      this.dialog = false;
    }
  }
};
</script>

<style>
.custom-loader {
  animation: loader 1s infinite;
  display: flex;
}
@-moz-keyframes loader {
  from {
    transform: rotate(0);
  }
  to {
    transform: rotate(360deg);
  }
}
@-webkit-keyframes loader {
  from {
    transform: rotate(0);
  }
  to {
    transform: rotate(360deg);
  }
}
@-o-keyframes loader {
  from {
    transform: rotate(0);
  }
  to {
    transform: rotate(360deg);
  }
}
@keyframes loader {
  from {
    transform: rotate(0);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>
