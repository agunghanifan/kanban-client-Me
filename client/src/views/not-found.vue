<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">Kanban Boards</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <input
                type="submit"
                class="btn btn-primary"
                id="btn-logout"
                value="Add task"
              />
            </li>
          </ul>
        </div>
      </div>
      <form class="d-flex">
        <input
          @click.prevent="logout"
          type="submit"
          class="btn btn-primary"
          id="btn-logout"
          value="Log Out"
        />
      </form>
    </nav>
    <div class="container">
      <h1>404 Page Not Found</h1>
    </div>
  </div>
</template>

<script>
export default {
  name: "notFoundPage",
  methods: {
    logOut() {
      this.$emit("logOut")
    }
  }
};
</script>

<style>
</style>