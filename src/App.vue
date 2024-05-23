<template>
  <q-layout view="lHh lpr lFf">
    <q-header elevated class="header">
      <q-toolbar>
        <q-toolbar-title>Toko Buku Online MS</q-toolbar-title>
        <q-space />
        <q-input
          v-model="search"
          debounce="300"
          placeholder="Cari buku..."
          @input="filterBooks"
          outlined
          dense
        ></q-input>
        <q-btn
          flat
          round
          dense
          icon="shopping_cart"
          @click="goToCartPage"
          class="header-btn"
        />
        <q-btn
          flat
          round
          dense
          icon="person"
          @click="goToProfilePage"
          class="header-btn"
        />
      </q-toolbar>
    </q-header>

    <q-page-container>
      <q-page padding class="page-background">
        <q-carousel
          animated
          v-model="slide"
          :autoplay="5000"
          arrows
          infinite
          class="carousel"
        >
          <q-carousel-slide
            :name="1"
            img-src="https://images.unsplash.com/photo-1512820790803-83ca734da794"
          />
          <q-carousel-slide
            :name="2"
            img-src="https://images.unsplash.com/photo-1524995997946-a1c2e315a42f"
          />
          <q-carousel-slide
            :name="3"
            img-src="https://images.unsplash.com/photo-1528747045269-390fe33c19d4"
          />
          <q-carousel-slide
            :name="4"
            img-src="https://images.unsplash.com/photo-1519681393784-d120267933ba"
          />
        </q-carousel>

        <q-tabs
          v-model="tab"
          class="q-mt-lg tabs"
          align="justify"
          indicator-color="primary"
        >
          <q-tab name="semua" label="Semua" />
          <q-tab name="fiksi" label="Fiksi" />
          <q-tab name="non-fiksi" label="Non-Fiksi" />
        </q-tabs>

        <q-tab-panels v-model="tab" animated>
          <q-tab-panel name="semua" class="tab-panel">
            <div class="q-gutter-md">
              <q-card
                v-for="book in filteredBooks"
                :key="book.title"
                class="q-ma-md book-card"
              >
                <q-card-section>
                  <q-img
                    :src="book.image"
                    class="product-image"
                    :alt="book.title"
                  />
                  <div class="product-details">
                    <div class="text-h6">{{ book.title }}</div>
                    <div class="text-subtitle2">Author: {{ book.author }}</div>
                    <div class="text-subtitle2">Genre: {{ book.genre }}</div>
                    <div class="text-h6">Price: Rp {{ book.price }}</div>
                    <q-rating
                      v-model="book.rating"
                      max="5"
                      size="20px"
                      color="amber"
                    />
                    <q-btn
                      color="primary"
                      label="Add to Cart"
                      @click="addToCart(book)"
                      class="add-to-cart-btn"
                    ></q-btn>
                  </div>
                </q-card-section>
              </q-card>
            </div>
          </q-tab-panel>

          <q-tab-panel name="fiksi" class="tab-panel">
            <div class="q-gutter-md">
              <q-card
                v-for="book in filteredBooks"
                :key="book.title"
                class="q-ma-md book-card"
              >
                <q-card-section>
                  <q-img
                    :src="book.image"
                    class="product-image"
                    :alt="book.title"
                  />
                  <div class="product-details">
                    <div class="text-h6">{{ book.title }}</div>
                    <div class="text-subtitle2">Author: {{ book.author }}</div>
                    <div class="text-subtitle2">Genre: {{ book.genre }}</div>
                    <div class="text-h6">Price: Rp {{ book.price }}</div>
                    <q-rating
                      v-model="book.rating"
                      max="5"
                      size="20px"
                      color="amber"
                    />
                    <q-btn
                      color="primary"
                      label="Add to Cart"
                      @click="addToCart(book)"
                      class="add-to-cart-btn"
                    ></q-btn>
                  </div>
                </q-card-section>
              </q-card>
            </div>
          </q-tab-panel>

          <q-tab-panel name="non-fiksi" class="tab-panel">
            <div class="q-gutter-md">
              <q-card
                v-for="book in filteredBooks"
                :key="book.title"
                class="q-ma-md book-card"
              >
                <q-card-section>
                  <q-img
                    :src="book.image"
                    class="product-image"
                    :alt="book.title"
                  />
                  <div class="product-details">
                    <div class="text-h6">{{ book.title }}</div>
                    <div class="text-subtitle2">Author: {{ book.author }}</div>
                    <div class="text-subtitle2">Genre: {{ book.genre }}</div>
                    <div class="text-h6">Price: Rp {{ book.price }}</div>
                    <q-rating
                      v-model="book.rating"
                      max="5"
                      size="20px"
                      color="amber"
                    />
                    <q-btn
                      color="primary"
                      label="Add to Cart"
                      @click="addToCart(book)"
                      class="add-to-cart-btn"
                    ></q-btn>
                  </div>
                </q-card-section>
              </q-card>
            </div>
          </q-tab-panel>
        </q-tab-panels>

        <div class="q-my-lg">
          <q-banner
            class="q-mb-lg promo-banner"
            color="purple-5"
            text-color="white"
            inline-actions
          >
            <div class="text-h6">Promo Hari Ini!</div>
            <div>Diskon 20% untuk semua buku baru.</div>
            <q-btn flat label="Lihat Promo" text-color="white" @click="lihatPromo" />
          </q-banner>
        </div>

        <div class="q-my-lg">
          <q-card class="q-ma-md testimoni-card">
            <q-card-section class="testimoni-section">
              <q-avatar icon="person" />
              <div class="testimoni-details">
                <div class="text-subtitle1">Ranti Putri</div>
                <div class="q-mt-sm">
                  "Buku-buku di toko ini sangat berkualitas dan pengirimannya cepat. Sangat
                  puas dengan layanan ini!"
                </div>
              </div>
            </q-card-section>
          </q-card>
          <q-card class="q-ma-md testimoni-card">
            <q-card-section class="testimoni-section">
              <q-avatar icon="person" />
              <div class="testimoni-details">
                <div class="text-subtitle1">Saqina Aksena</div>
                <div class="q-mt-sm">
                  "Pengalaman belanja buku online yang luar biasa. Koleksi bukunya sangat
                  lengkap dan harga terjangkau."
                </div>
              </div>
            </q-card-section>
          </q-card>
        </div>
      </q-page>
    </q-page-container>

    <q-footer elevated class="footer">
      <q-toolbar>
        <q-toolbar-title>@toko_buku_online</q-toolbar-title>
        <q-space />
        <q-btn flat round dense icon="phone" @click="contactUs" />
      </q-toolbar>
    </q-footer>
  </q-layout>
</template>

<script>
import { ref, watch } from "vue";

export default {
  setup() {
    const slide = ref(1);
    const tab = ref("semua");
    const search = ref("");
    const books = ref([
      {
        title: "Buku A",
        author: "Pengarang A",
        genre: "Fiksi",
        price: 100000,
        image:
          "https://images.unsplash.com/photo-1512820790803-83ca734da794",
        rating: 4.5,
      },
      {
        title: "Buku B",
        author: "Pengarang B",
        genre: "Non-Fiksi",
        price: 150000,
        image:
          "https://images.unsplash.com/photo-1524995997946-a1c2e315a42f",
        rating: 4.7,
      },
      {
        title: "Buku C",
        author: "Pengarang C",
        genre: "Fiksi",
        price: 80000,
        image:
          "https://images.unsplash.com/photo-1528747045269-390fe33c19d4",
        rating: 4.2,
      },
      {
        title: "Buku D",
        author: "Pengarang D",
        genre: "Non-Fiksi",
        price: 120000,
        image:
          "https://images.unsplash.com/photo-1519681393784-d120267933ba",
        rating: 4.8,
      },
    ]);
    const filteredBooks = ref([...books.value]);

    watch(search, (newSearch) => {
      if (newSearch === "") {
        filteredBooks.value = books.value;
      } else {
        filteredBooks.value = books.value.filter((book) =>
          book.title.toLowerCase().includes(newSearch.toLowerCase())
        );
      }
    });

    const filterBooks = () => {
      filteredBooks.value = books.value.filter((book) =>
        book.title.toLowerCase().includes(search.value.toLowerCase())
      );
    };

    const addToCart = (book) => {
      // Logika untuk menambahkan buku ke keranjang
      console.log("Added to cart:", book);
    };

    const goToCartPage = () => {
      // Logika untuk menuju halaman keranjang
      console.log("Navigating to cart page");
    };

    const goToProfilePage = () => {
      // Logika untuk menuju halaman profil
      console.log("Navigating to profile page");
    };

    const lihatPromo = () => {
      // Logika untuk melihat promo
      console.log("Viewing promo");
    };

    const contactUs = () => {
      // Logika untuk menghubungi kami
      console.log("Contacting us");
    };

    return {
      slide,
      tab,
      search,
      books,
      filteredBooks,
      filterBooks,
      addToCart,
      goToCartPage,
      goToProfilePage,
      lihatPromo,
      contactUs,
    };
  },
};
</script>

<style scoped>
.header {
  background-color: #6200ea;
  color: white;
}
.header-btn {
  color: white;
}
.page-background {
  background-color: #f5f5f5;
}
.carousel {
  margin-bottom: 20px;
}
.tabs {
  margin-top: 20px;
}
.tab-panel {
  margin-top: 20px;
}
.book-card {
  display: flex;
  flex-direction: column;
  width: 300px;
}
.product-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}
.product-details {
  margin-top: 10px;
}
.add-to-cart-btn {
  margin-top: 10px;
}
.promo-banner {
  padding: 20px;
}
.testimoni-card {
  padding: 20px;
}
.testimoni-section {
  display: flex;
  align-items: center;
}
.testimoni-details {
  margin-left: 10px;
}
.footer {
  background-color: #6200ea;
  color: white;
}
</style>