<script lang="ts">
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher<{ inputChange: string }>();
  export let id: string;
  export let type: "url" | "email" | "text" | "number" | null = null;
  export let rows: number | null = null;
  export let required: boolean | undefined;
  export let name: string;
  export let value: string;
  export let inputType: "input" | "textarea";
</script>

<div class="form-control">
  <label for={id}>{name}</label>
  {#if inputType === "textarea"}
    <textarea
      {rows}
      {id}
      on:input={(e) => dispatch("inputChange", e.currentTarget.value)}
      {required}>{value}</textarea
    >
  {:else}
    <input
      {type}
      {id}
      {value}
      on:input={(e) => dispatch("inputChange", e.currentTarget.value)}
      {required}
    />
  {/if}
</div>

<style>
  input,
  textarea {
    display: block;
    width: 100%;
    font: inherit;
    border: none;
    border-bottom: 2px solid #ccc;
    border-radius: 3px 3px 0 0;
    background: white;
    padding: 0.15rem 0.25rem;
    transition: border-color 0.1s ease-out;
  }

  input:focus,
  textarea:focus {
    border-color: #e40763;
    outline: none;
  }

  label {
    display: block;
    margin-bottom: 0.5rem;
    width: 100%;
  }

  .form-control {
    padding: 0.5rem 0;
    width: 100%;
    margin: 0.25rem 0;
  }
</style>
