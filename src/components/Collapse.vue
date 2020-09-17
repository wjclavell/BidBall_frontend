<template>
  <section>
    <b-collapse :open="false" aria-id="contentIdForA11y1">
      <button
        class="button is-primary"
        slot="trigger"
        aria-controls="contentIdForA11y1"
      >
        Click me!
      </button>
      <div class="notification">
        <div class="content">
          <h3>
            Example
          </h3>
          <p>
            This will be the user profile page.<br />It'll include the user's
            profile pic, info, coin balance, record of correct/incorrect, and
            the ability to update their favorite sport/teams<br />also the
            ability to delete their account.
          </p>
        </div>
      </div>
    </b-collapse>
  </section>
</template>

<script>
export default {
  name: "Collapse",
};
</script>
