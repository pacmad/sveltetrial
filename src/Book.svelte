<script>
  const slots = [
    { row: "a", seats: 12 },
    { row: "b", seats: 12 },
    { row: "c", seats: 12 },
    { row: "d", seats: 12 },
    { row: "e", seats: 12 },
    { row: "f", seats: 12 },
    { row: "g", seats: 12 },
    { row: "h", seats: 12 },
    { row: "i", seats: 12 },
    { row: "j", seats: 12 }
  ];
  const movie = {
    title: "John Wick: Chapter 3 – Parabellum",
    year: "2019",
    dur: 239
  };
  let i = 0;
  const src = "../favicon.png";
  let cart = [];
  let purchased = [];

  const addToCart = args => {
    if (purchased.includes(args)) {
      alert("Seats have been booked!");
      return;
    }

    if (cart.indexOf(args) === -1) {
      cart = [...cart, args];
    } else {
      cart = cart.filter(val => val != args);
    }

    if (cart.length > 4) {
      alert("Maximum 4 (four) seats only!");
      return cart = cart.filter(val => val != args);
    }
   
  };

  const purchase = () => {
    if (purchased.indexOf(...cart) === -1) {
      purchased = [...purchased, ...cart];
    }

    cart = [];
    return alert("Thank you for booking!");
  };

  $: bill = 35000 * cart.length;
  $: total = slots.reduce((acc, val) => (acc += val.seats), i);
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  .container {
    display: grid;
    grid-gap: 0.4rem;
    grid-template-columns: 9fr 3fr;
    grid-template-rows: repeat(2, minmax(100, 1fr));
    height: 100vh;
    color: #fff;
    background-color: #edecde;
  }

  button {
    margin-top: 5px;
    background-color: transparent;
    border: 0.3rem solid #cecbab;
    padding: 0.5rem;
    cursor: pointer;
    color: #cecbab;
  }
  .row {
    display: grid;
    grid-column: 1/2;
    grid-gap: 0.4rem;
    grid-template-columns: repeat(14, 1fr);
  }

  .take {
    display: grid;
    font-size: 1rem !important;
    align-items: center;
    justify-items: center;
    background-color: #30422e;
    cursor: pointer;
  }

  .bold {
    text-transform: uppercase;
    font-weight: 500;
    font-size: 1.6rem;
  }

  .dark {
    color: #edecde;
    background-color: #30422e;
  }

  .info {
    grid-column: 2/-1;
    grid-row: 1/12;
    font-size: 2rem;
    text-transform: capitalize;
    font-weight: 500;
    color: #edecde;
    grid-template-rows: repeat(4, 1fr);
  }

  .rows {
    grid-row: 1/-1;
    grid-column: 7/9;
    background-color: #fff;
    color: #2e2e2e;
  }

  .d-grid {
    display: grid;
    grid-template-columns: 1fr;

    place-items: center center;
  }

  .layar {
    background-color: #2e2e2e;
    grid-column: 1/2;
    letter-spacing: 1rem;
  }

  .seat {
    cursor: pointer;
    display: grid;
    font-size: 1rem !important;
    align-items: center;
    justify-items: center;
    background-color: #cecbab;
  }

  .sub {
    font-size: 1rem;
    font-weight: 400;
  }

  .logo {
    background-size: cover;
    object-fit: cover;
  }

  .info img {
    height: auto;
    width: 50%;
  }

  .pulse {
    box-shadow: 0 0 0 0 rgba(48, 66, 46, 0.938);
    -webkit-animation: pulse 1.25s infinite cubic-bezier(0.66, 0, 0, 1);
    -moz-animation: pulse 1.25s infinite cubic-bezier(0.66, 0, 0, 1);
    -ms-animation: pulse 1.25s infinite cubic-bezier(0.66, 0, 0, 1);
    animation: pulse 1.25s infinite cubic-bezier(0.66, 0, 0, 1);
  }

  @-webkit-keyframes pulse {
    to {
      box-shadow: 0 0 0 18px rgba(232, 76, 61, 0);
    }
  }
  @-moz-keyframes pulse {
    to {
      box-shadow: 0 0 0 18px rgba(232, 76, 61, 0);
    }
  }
  @-ms-keyframes pulse {
    to {
      box-shadow: 0 0 0 18px rgba(232, 76, 61, 0);
    }
  }
  @keyframes pulse {
    to {
      box-shadow: 0 0 0 18px rgba(232, 76, 61, 0);
    }
  }
</style>

<div class="container">
  {#each slots as { row, seats }}
    <div class="row">
      <div class="rows dark bold">
        <div class="d-grid">{row}</div>
      </div>
      {#each { length: seats } as _, i}
        <div
          on:click={() => addToCart(row + (i + 1))}
          class={!cart.includes(row + (i + 1)) && !purchased.includes(row + (i + 1)) ? 'seat bold' : purchased.includes(row + (i + 1)) ? 'take bold' : 'take pulse bold'}>
           {row}{i + 1}
        </div>
      {/each}
    </div>
  {/each}
  <div class="info d-grid dark">
    <div class="bill d-grid">
      <h4>Bill</h4>
      <p class="sub"> {cart.length === 0 ? '' : `[${cart}]`} </p>
      <p class="sub">
         {cart.length > 0 ? `${cart.length} x 35.000 = ${bill} ` : ''}
      </p>
    </div>
    <div class="movie-info d-grid">
      <h4>info</h4>
      <img {src} alt="logo" class="logo" />
    </div>
    <div class="purchase d-grid" on:click={purchase}>
      <h4>purchase</h4>
      <button class="bold sub" on:click={purchased}>purchase</button>
    </div>
    <div class="total-seats d-grid">
      <h4>seats</h4>
      <p class="sub">
         {total - purchased.length === 0 ? 'full booked' : `${total - purchased.length}`}

      </p>
    </div>
  </div>
  <div class="layar bold d-grid">screen</div>
</div>
