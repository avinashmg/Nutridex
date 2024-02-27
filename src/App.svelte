<script>
  import { onMount } from "svelte";
  let list = [];
  let per_calorie = false;
  onMount(async () => {
    const response = await fetch("/foods.json");
    list = await response.json();
  });

  const calories = (item) => {
    return item.protein * 4 + item.carbohydrates * 4 + item.fats * 9;
  };
  const round = (n, p = 2) => ((e) => Math.round(n * e) / e)(Math.pow(10, p));
</script>

<div class="nutridex">
  <div class="topbar">NUTRIDEX</div>
  <div class="topbar-spacer"></div>
  <div class="body">
    <div class="sidebar">
      <div class="search">
        <input type="text" name="" id="" />
      </div>
      <div class="option">
        <input type="checkbox" bind:checked={per_calorie} /> Per 100 Calories
      </div>
    </div>
    <div class="list">
      {#each list as item}
        <div class="item">
          <div class="image">
            <img src={item.img} alt="" />
          </div>
          <h2>{item.name}</h2>
          <div class="info">
            <h4 style="color: #555;">
              MACROS
              {#if per_calorie == false}
                <small>Per 100g</small>
              {:else}
                <small>Per 100 Kcl</small>
              {/if}
            </h4>
            {#if per_calorie == false}
              <div>Protein : {item.protein}g</div>
              <div>Carbohydrates: {item.carbohydrates}g</div>
              <div>Fats: {item.fats}g</div>
              <div>Calories: {calories(item)}Kcl</div>
            {:else}
              <div>
                Protein: {round((item.protein / calories(item)) * 100)}g
                <div>
                  Carbohydrates: {round(
                    (item.carbohydrates / calories(item)) * 100
                  )}g
                </div>
                <div>Fats: {round((item.fats / calories(item)) * 100)}g</div>
                <div>Weight: {round((100 / calories(item)) * 100)}g</div>
              </div>
            {/if}
          </div>
        </div>
      {/each}
    </div>
  </div>
</div>

<style scoped>
  .nutridex {
    min-height: 100vh;
    background-color: rgb(255, 255, 255);
  }
  .topbar {
    height: 60px;
    background: #232323;
    border-bottom: solid 5px;
    border-color: rgb(187, 0, 0);
    color: #ffffff;
    font-size: 30px;
    text-align: center;
    font-weight: 600;
    line-height: 60px;
    position: fixed;
    width: 100%;
  }
  .topbar-spacer {
    height: 60px;
  }
  .body {
    display: flex;
  }
  .list {
    display: flex;
    margin: 10px;
    color: #232323;
    flex-wrap: wrap;
    justify-content: center;
  }
  .item {
    box-shadow: #2333 0 0 20px 10px;
    padding: 10px;
    border-radius: 20px;
    margin: 10px;
  }
  .info {
    outline: #232323 solid 1px;
    border-radius: 10px;
    padding: 10px;
    background-color: #ffdf98;
  }
  .image {
    margin-bottom: 10px;
    height: 200px;
    overflow: hidden;
    border-radius: 10px;
    outline: solid #232323 1px;
    width: 200px;
  }
  .image > img {
    width: 100%;
    min-height: 100%;
  }
  .sidebar {
    background-color: #232323;
    min-height: calc(100vh - 60px);
  }
  .option {
    display: flex;
    align-items: center;
    margin: 10px;
  }
  .option > input {
    width: 15px;
  }
</style>
