<template>
  <header>
    <div class="container">
      <h1 class="logo">Practice #1</h1>
      <nav>
        <!--<router-link to="/">Home</router-link> |-->
        <!--<router-link to="/about">About</router-link>-->
      </nav>
    </div>
  </header>
  <router-view/>
</template>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* Fuente y colores base */
body {
  font-family: 'Segoe UI', sans-serif;
  color: #222;
  background: #fff;
  line-height: 1.6;
}

.container {
  max-width: 1100px;
  margin: 0 auto;
  padding: 1.5rem;
}


/* Header */
header {
  background: #f8f8f8;
  border-bottom: 1px solid #ddd;
}

.logo {
  font-size: 1.8rem;
  font-weight: bold;
}

header .container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
}

nav {
  display: flex;
  gap: 1rem;
}

nav a {
  text-decoration: none;
  color: #333;
  font-weight: 500;
}

@media (max-width: 768px) {
  header .container {
    flex-direction: column;
    align-items: flex-start;
  }

  nav {
    flex-direction: column;
    gap: 0.5rem;
    margin-top: 1rem;
  }

}

</style>
