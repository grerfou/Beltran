<template>
  <div class="menu-container">
    <div class="left-column">
      <div class="vertical-wrapper">
        <span class="vertical-text">Contact</span>
        <div class="circle_de"></div>
      </div>
    </div>
    <div class="right-column">
      <div class="vertical-wrapper">
        <span class="vertical-text">Work</span>
        <div class="circle_un"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Menu',
};
</script>

<style scoped>
.menu-container {
  position: fixed;
  top: 0;
  right: 0;
  height: 100vh;
  display: flex;
  flex-direction: row-reverse;
  font-family: "Barlow-Condesed-R";
}

.left-column,
.right-column {
  width: 3vw;
  height: 100%;
  display: flex;
  align-items: start;
  padding-top: 1rem;
  justify-content: center;
  background-color: rgb(52, 52, 52);
  border: 1px solid rgb(194, 194, 194);
}

/* Conteneur du cercle + texte */
.vertical-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Cercle décoratif */
.circle_un {
  width: 10px;
  height: 10px;
  background-color: rgb(194, 194, 194);
  border-radius: 50%;
  margin-top: 10px;
}

.circle_de {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  margin-top: 10px;
  border: 1px solid rgb(194, 194, 194);
}

/* Texte vertical */
.vertical-text {
  writing-mode: vertical-rl;
  transform: rotate(180deg);
  color: rgb(194, 194, 194);
  font-size: 2.2rem;
  font-weight: bold;
  letter-spacing: 0.1rem;
}

/* --- MOBILE --- */
@media screen and (max-width: 768px) {
  .menu-container {
    position: fixed;
    top: 0;
    right: 0;
    left: 0;
    height: 60px;
    width: 100%;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    padding: 0 2rem;
    border-bottom: 1px solid rgb(194, 194, 194);
    background-color: rgb(52, 52, 52);
  }

  .left-column,
  .right-column {
    width: auto;
    height: auto;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: flex-start;
    padding: 0;
    border: none;
    background: none;
  }

  .vertical-wrapper {
    flex-direction: row;
    gap: 0.5rem;
  }

  .circle_un,
  .circle_de {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    background-color: rgb(194, 194, 194);
    border: none;
    margin-top: 0;
  }

  .circle_de {
    background: none;
    border: 1px solid rgb(194, 194, 194);
  }

  .vertical-text {
    writing-mode: horizontal-tb;
    transform: none;
    font-size: 1.5rem;
  }
}
</style>

