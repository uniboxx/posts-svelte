<script>
  let {children,modalIsVisible=$bindable()}=$props();
</script>

<div role="button" tabindex="0" class="backdrop" onclick={()=>modalIsVisible=false} onkeyup={e => {
  }} ></div>
<dialog class="modal" open={modalIsVisible}>
  {@render children()}
</dialog>

<style lang="scss">
  @import '../styles/vars';

  .backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.6);
  z-index: 1;
}

.modal {
  border: none;
  border-radius: 6px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
  padding: 0;
  overflow: hidden;
  z-index: 1;
}

</style>
