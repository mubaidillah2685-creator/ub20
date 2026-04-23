<script>
  import { onMount } from 'svelte';

  let comments = [];
  let name = '';
  let message = '';

  async function loadComments() {
    const res = await fetch('/api/comments');
    comments = await res.json();
  }

  async function addComment() {
    if (!name || !message) return;

    await fetch('/api/comments', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ name, message })
    });

    name = '';
    message = '';
    loadComments();
  }

  async function deleteComment(id) {
    await fetch(`/api/comments/${id}`, {
      method: 'DELETE'
    });
    loadComments();
  }

  onMount(loadComments);

  const menuBakso = [
    { nama: 'Bakso Urat ', harga: '18.000' },
    { nama: 'Bakso Mercon', harga: '22.000' },
    { nama: 'Bakso Jumbo Mercon', harga: '28.000' }
  ];
</script>

<style>
  :global(body) {
    margin: 0;
    font-family: 'Segoe UI', sans-serif;
    background: #111;
    color: white;
  }

  .hero {
    height: 90vh;
    background: linear-gradient(rgba(0,0,0,.65), rgba(0,0,0,.65)),
      url('https://images.unsplash.com/photo-1601050690597-df0568f70950?auto=format&fit=crop&w=1500&q=80');
    background-size: cover;
    background-position: center;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    padding: 20px;
  }

  .hero-content h1 {
    font-size: 4rem;
    color: #ffd700;
    margin-bottom: 10px;
  }

  .hero-content p {
    font-size: 1.3rem;
    color: #eee;
  }

  .section {
    max-width: 1100px;
    margin: auto;
    padding: 60px 20px;
  }

  .section-title {
    text-align: center;
    font-size: 2rem;
    color: #ffd700;
    margin-bottom: 30px;
  }

  .menu-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
  }

  .menu-card {
    background: linear-gradient(145deg, #1c1c1c, #2a2a2a);
    padding: 25px;
    border-radius: 16px;
    box-shadow: 0 10px 25px rgba(255, 215, 0, 0.15);
    transition: transform 0.3s ease;
  }

  .menu-card:hover {
    transform: translateY(-8px);
  }

  .menu-card h3 {
    color: #ffd700;
    margin-bottom: 10px;
  }

  .price {
    font-size: 1.2rem;
    font-weight: bold;
  }

  .about {
    text-align: center;
    color: #ddd;
    line-height: 1.8;
    max-width: 700px;
    margin: auto;
  }

  .comment-form {
    display: flex;
    flex-direction: column;
    gap: 12px;
    margin-bottom: 25px;
  }

  input, textarea {
    padding: 14px;
    border-radius: 10px;
    border: none;
    outline: none;
    font-size: 1rem;
  }

  button {
    background: linear-gradient(45deg, #ffd700, #ff9800);
    color: #111;
    font-weight: bold;
    padding: 14px;
    border: none;
    border-radius: 10px;
    cursor: pointer;
    transition: 0.3s;
  }

  button:hover {
    opacity: 0.9;
    transform: scale(1.02);
  }

  .comment-item {
    background: #1c1c1c;
    padding: 18px;
    margin-top: 15px;
    border-radius: 12px;
    box-shadow: 0 6px 15px rgba(255, 215, 0, 0.08);
  }

  .comment-item strong {
    color: #ffd700;
  }

  .delete-btn {
    margin-top: 10px;
    background: #e53935;
    color: white;
  }
</style>

<section class="hero">
  <div class="hero-content">
    <h1>Bakso Mercon </h1>
    <p>Kenikmatan Bakso Premium dengan Rasa Pedes Nampol Bangett</p>
  </div>
</section>

<section class="section">
  <h2 class="section-title">Menu Favorit</h2>
  <div class="menu-grid">
    {#each menuBakso as item}
      <div class="menu-card">
        <h3>{item.nama}</h3>
        <div class="price">Rp {item.harga}</div>
      </div>
    {/each}
  </div>
</section>

<section class="section">
  <h2 class="section-title">Tentang Kami</h2>
  <p class="about">
    Bakso Mercon menghadirkan bakso berkualitas premium dengan bahan pilihan,
    kuah kaya rasa, mehadirkan menu varian utama yaitu bakso mercon dan pengalaman kuliner yang elegan. Cocok untuk semua
    pecinta bakso yang ingin menikmati cita rasa berkelas.
  </p>
</section>

<section class="section">
  <h2 class="section-title">Komentar Pelanggan</h2>

  <div class="comment-form">
    <input bind:value={name} placeholder="Nama Anda" />
    <textarea bind:value={message} placeholder="Tulis komentar..."></textarea>
    <button on:click={addComment}>Kirim Komentar</button>
  </div>

  {#each comments as c}
    <div class="comment-item">
      <strong>{c.name}</strong>
      <p>{c.message}</p>
      <button class="delete-btn" on:click={() => deleteComment(c.id)}>
        Hapus
      </button>
    </div>
  {/each}
</section>