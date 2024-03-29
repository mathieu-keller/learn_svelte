<script lang="ts">
  import meetupStore from "../MeetupStore";
  import Button from "../../UI/Button.svelte";
  import Badge from "../../UI/Badge.svelte";
  import { createEventDispatcher } from "svelte";
  import { updateMeetup } from "../../Rest/Http";

  export let objectId: string;
  export let address: string;
  export let contactEmail: string;
  export let subtitle: string;
  export let imageUrl: string;
  export let description: string;
  export let title: string;
  export let isFavorite: boolean;

  const setFav = (objectId: string) => {
    const meet = $meetupStore.find((meetup) => meetup.objectId === objectId);
    if (meet !== undefined) {
      updateMeetup({
        objectId: meet.objectId,
        address: meet.address,
        contactEmail: meet.contactEmail,
        subtitle: meet.subtitle,
        imageUrl: meet.imageUrl,
        description: meet.description,
        title: meet.title,
        isFavorite: !meet.isFavorite,
      });
    }
  };

  const dispatch =
    createEventDispatcher<{ openDetail: string; edit: string }>();
</script>

<article id={objectId}>
  <header>
    <h1>
      {title}
      {#if isFavorite}
        <Badge>Favorite</Badge>
      {/if}
    </h1>
    <h2>{subtitle}</h2>
    <h3>{address}</h3>
  </header>
  <div class="image">
    <img src={imageUrl} alt="meet-place" />
  </div>
  <div class="content">
    <p>{description}</p>
  </div>
  <footer>
    <Button type="button" on:click={() => dispatch("edit", objectId)}
      >Edit</Button
    >
    <Button href="mailto:{contactEmail}">Contact</Button>
    <Button type="button" on:click={() => dispatch("openDetail", objectId)}>
      Show Details
    </Button>
    <Button
      type="button"
      mode="outline"
      color={isFavorite ? null : "success"}
      on:click={() => setFav(objectId)}
    >
      {isFavorite ? "un-favorite" : "favorite"}
    </Button>
  </footer>
</article>

<style>
  article {
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
    border-radius: 5px;
    background: white;
    margin: 1rem;
  }

  header,
  .content,
  footer {
    padding: 1rem;
  }

  .image {
    width: 100%;
    height: 14rem;
  }

  .image img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  h1 {
    font-size: 1.25rem;
    margin: 0.5rem 0;
    font-family: "Roboto Slab", sans-serif;
  }

  h2 {
    font-size: 1rem;
    color: #808080;
    margin: 0.5rem 0;
  }

  p {
    font-size: 1.25rem;
    margin: 0;
  }

  div {
    text-align: right;
  }
</style>
