<script setup>
import { ref, onMounted } from "vue";
const { data } = await useAsyncData("market2", () =>
    $fetch(
        `https://api.dev.inoutdelivery.com.co/v1/banners?business=ppc.inoutdelivery.com`
    )
);
const img = data._rawValue[0].banners.map(banner => banner.banner.image.lg);
const imgIndex = ref(0);

const indexValidator = () => {
    imgIndex.value = (imgIndex.value + img.length) % img.length;
};


const prevImage = () => {

    imgIndex.value--;
    indexValidator();
}

const nextImage = () => {

    imgIndex.value++;
    indexValidator();
}

const paginationSelect = computed(() => {
    return (index) => {
        if (imgIndex.value === index) {
            return "active";
        } else if (imgIndex.value === 0 && index === undefined) {
            return "active";
        } else {
            return "";
        }
    };
});

onMounted(() => {
    setInterval(() => {
        nextImage();
    }, 5000);
});
</script>

<template>
    <div class="carousel">
        <transition name="slide">
            <img :src="img[imgIndex]" :key="imgIndex" class="img-responsive">
        </transition>

        <div class="buttons">
            <i @click="prevImage" class="animate__animated animate__fadeIn "><svg xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 256 512"><!--! Font Awesome Pro 6.3.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. -->
                    <path
                        d="M9.4 233.4c-12.5 12.5-12.5 32.8 0 45.3l160 160c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L77.3 256 214.6 118.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0l-160 160z" />
                </svg></i>

            <i @click="nextImage" class="animate__animated animate__fadeIn "><svg xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 256 512"><!--! Font Awesome Pro 6.3.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. -->
                    <path
                        d="M246.6 233.4c12.5 12.5 12.5 32.8 0 45.3l-160 160c-12.5 12.5-32.8 12.5-45.3 0s-12.5-32.8 0-45.3L178.7 256 41.4 118.6c-12.5-12.5-12.5-32.8 0-45.3s32.8-12.5 45.3 0l160 160z" />
                </svg></i>
        </div>

        <div class="gallery-pagination">
            <ul>
                <li v-for="(image, index) in img" :key="index">
                    <button @click="imgIndex = index" :class="paginationSelect(index)"></button>
                </li>

            </ul>
        </div>
    </div>
</template>

<style lang="scss" scoped >
.carousel {
    display: block;
    position: relative;
    width: 100%;
    aspect-ratio: 4/2;
    //background-image: url("https://cdn.inoutdelivery.com/ppc.inoutdelivery.com/banner.jpg");
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    max-height: 70vh;
}

.img-responsive {
    display: block;
    width: 100%;
    /* la imagen ocupa todo el ancho del div */
    height: 100%;
    /* la imagen ocupa todo el alto del div */
    max-width: 100%;
    max-height: 100%;
    margin: auto;
    /* la imagen se ajusta al tamaño del contenedor sin recortarse */
    object-position: top;

}



/* animation slide */

.slide-enter-active {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    animation: slide-in 0.5s ease forwards;
}

.slide-leave-active {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    animation: slide-out 0.5s ease forwards;
}

.slide-enter,
.slide-leave-to {
    opacity: 0;
}

@keyframes slide-in {
    0% {
        opacity: 0;
        transform: translateX(100%);
    }

    100% {
        opacity: 1;
        transform: translateX(0);
    }
}

@keyframes slide-out {
    0% {
        opacity: 1;
        transform: translateX(0);
    }

    100% {
        opacity: 0;
        transform: translateX(-100%);
    }
}


@keyframes slide-in-right {
    0% {
        opacity: 0;
        transform: translateX(-100%);
    }

    100% {
        opacity: 1;
        transform: translateX(0);
    }
}

@keyframes slide-out-right {
    0% {
        opacity: 1;
        transform: translateX(0);
    }

    100% {
        opacity: 0;
        transform: translateX(100%);
    }
}


/* end animation slide */


.buttons {
    position: absolute;
    left: 0;
    right: 0;
    top: 50%;
    transform: translateY(-50%);
    bottom: 0;
    display: flex;
    height: 50px;
    justify-content: space-between;
    align-items: flex-start;

}

.buttons i {
    font-size: 1rem;
    padding: 0.5rem 1rem;
    background-color: rgba(2, 2, 2, 0.5);
    color: #fff;
    border: none;
    border-radius: 50%;
    cursor: pointer;
    fill: #fff;
    width: 45px;
}

.buttons i:hover {
    background-color: rgba(59, 59, 59, 0.5);
}

.gallery-pagination {
    background-color: rgba(97, 97, 97, 0.5);
    position: absolute;
    bottom: 0;
    display: flex;
    justify-content: space-around;
    align-items: center;
    ;
    width: 100%;
    aspect-ratio: 25/1;
}

.gallery-pagination ul {
    list-style-type: none;
    display: flex;
    margin: 0;
    padding: 0;
}

.gallery-pagination li {
    width: 1.5rem;
    height: 1.5rem;
    display: flex;
    justify-content: center;
    align-items: center;
    border: none;
    border-radius: 100%;
    cursor: pointer;
    margin: 0 5px;
    background-color: transparent;
}

.gallery-pagination li button {
    font-size: 1rem;
    width: 1.5rem;
    height: 1.5rem;
    display: flex;
    justify-content: center;
    align-items: center;
    border: none;
    border-radius: 100%;
    cursor: pointer;
    background-color: #fff;

}

.gallery-pagination li button:hover {
    background-color: #d6d4d4;

}

.gallery-pagination li button.active {
    background-color: #7e7b7b;
}


/* Mobile styles */

@media (max-width: 767px) {
    .carousel {
        width: 100%;

    }

    .buttons {
        padding: 0.5rem;
    }

    .buttons i {
        font-size: 0.8rem;
        padding: 0.25rem 0.5rem;
        width: 20px;
    }

    .gallery-pagination {
        padding: 0.25rem;
        height: auto;
    }

    .gallery-pagination li {
        width: auto;
        height: auto;
    }

    .gallery-pagination li button {
        font-size: 0.8rem;
        width: 10px;
        height: 10px;

    }
}

/* Tablet styles */
@media (min-width: 768px) and (max-width: 1023px) {

    .buttons i {
        font-size: 1rem;
        padding: 0.5rem 1rem;
    }

    .gallery-pagination {
        padding: 0.5rem;
    }

    .gallery-pagination li {
        width: 1.5rem;
        height: 1.5rem;
        font-size: 1rem;
    }

    .gallery-pagination li button {
        font-size: 1rem;
        width: 1.5rem;
        height: 1.5rem;
    }
}
</style>

