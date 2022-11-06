<template>
  <div>
    <teleport to="body">
      <div @click="$emit('close')">
        <dialog open>
          <header>
            <slot name="header">
              <img
                class="pokeball-img"
                src="../assets/pokeball.png"
                alt="pokemon ball image"
              />
              <h2>
                {{ title }}
              </h2>
            </slot>
          </header>
          <section>
            <slot
              ><p class="result">Score: {{ result }}/10</p>
            </slot>
          </section>
          <menu>
            <slot name="actions"></slot>
          </menu>
        </dialog>
      </div>
    </teleport>
  </div>
</template>

<script>
export default {
  props: ["title", "result"],
};
</script>

<style scoped>
div {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100%;
  background-color: rgb(231, 185, 1);
  z-index: 10;
}

dialog {
  position: fixed;
  top: 10px;
  left: 10%;
  width: 80%;
  z-index: 100;
  border-radius: 12px;
  border: none;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 0;
  margin: 0;
  overflow: hidden;
}

header {
  background-color: rgb(234, 0, 17);
  color: white;
  width: 100%;
  padding: 1rem;
}

header h2 {
  margin: 0;
}

section {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
}

menu {
  padding: 1rem;
  display: flex;
  justify-content: flex-end;
  margin: 0;
}

.result {
  font-size: 2rem;
}

.pokeball-img {
  height: 40px;
}

@media (min-width: 768px) {
  dialog {
    top: 20px;
    left: calc(50% - 20rem);
    width: 40rem;
  }
}
</style>
