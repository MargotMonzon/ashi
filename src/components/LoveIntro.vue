<script setup>
import { onBeforeUnmount, onMounted, ref } from 'vue'

const sectionRef = ref(null)

const isVisible = ref(false)

let observer = null

onMounted(() => {
    observer = new IntersectionObserver(
        ([entry]) => {
            if (entry.isIntersecting) {
                isVisible.value = true

                observer?.disconnect()
            }
        },
        {
            threshold: 0.3,
        },
    )

    if (sectionRef.value) {
        observer.observe(sectionRef.value)
    }
})

onBeforeUnmount(() => {
    observer?.disconnect()
})
</script>

<template>
    <section ref="sectionRef" class="love-intro">
        <div class="love-intro__content" :class="{
            'love-intro__content--visible': isVisible,
        }">
            <span class="love-intro__eyebrow">
                antes de continuar...
            </span>

            <div class="love-intro__heart">
                ♡
            </div>

            <h2 class="love-intro__title">
                Quiero decirte
                <span>algo pequeñito.</span>
            </h2>

            <div class="love-intro__letter">
                <span class="love-intro__quote">
                    “
                </span>

                <p>
                    Como siempre empieza todo lo que te digo, te amo
                    y te amo mucho mas de lo que puedas imaginar. Te amo con mi vida,
                    eres todo lo que siempre quise.
                </p>

                <p>
                    La Goti chiquita y la Goti adulta chiquita estan demasiado
                    felices de que podemos compartir muchos momentos unicos juntas,
                    incluso haciendo nada, todo es especial.
                </p>

                <span class="love-intro__signature">
                    — Goti ♡
                </span>
            </div>

            <div class="love-intro__continue">
                <span>
                    pero esto recién comienza...
                </span>

                <span class="love-intro__arrow">
                    ↓
                </span>
            </div>
        </div>

        <span class="love-intro__decoration love-intro__decoration--one">
            ♡
        </span>

        <span class="love-intro__decoration love-intro__decoration--two">
            ✦
        </span>

        <span class="love-intro__decoration love-intro__decoration--three">
            ♡
        </span>
    </section>
</template>

<style scoped>
.love-intro {
    position: relative;

    display: flex;
    align-items: center;
    justify-content: center;

    min-height: 100dvh;

    padding: 120px 24px;

    overflow: hidden;

    background:
        radial-gradient(circle at 50% 50%,
            rgba(255, 255, 255, 0.9),
            transparent 45%),
        linear-gradient(180deg,
            #f9e7e9 0%,
            #fffaf8 45%,
            #fffaf8 100%);
}

.love-intro__content {
    position: relative;

    z-index: 2;

    width: 100%;
    max-width: 650px;

    text-align: center;

    opacity: 0;

    transform: translateY(50px);

    transition:
        opacity 1.3s ease,
        transform 1.3s ease;
}

.love-intro__content--visible {
    opacity: 1;

    transform: translateY(0);
}

.love-intro__eyebrow {
    display: inline-block;

    margin-bottom: 30px;

    font-size: 10px;
    font-weight: 600;

    letter-spacing: 0.28em;
    text-transform: uppercase;

    color: var(--red);
}

.love-intro__heart {
    margin-bottom: 20px;

    font-family: 'Cormorant Garamond', serif;

    font-size: 32px;

    color: var(--pink);

    animation: heartbeat 2.4s ease-in-out infinite;
}

.love-intro__title {
    font-family: 'Cormorant Garamond', serif;

    font-size: clamp(48px, 10vw, 78px);
    font-weight: 500;

    line-height: 0.92;
    letter-spacing: -0.03em;

    color: var(--wine);
}

.love-intro__title span {
    display: block;

    font-style: italic;

    color: var(--red);
}

/* ============================= */
/* CARTITA */
/* ============================= */

.love-intro__letter {
    position: relative;

    max-width: 520px;

    margin: 55px auto 0;

    padding: 48px 42px;

    border:
        1px solid rgba(169, 74, 90, 0.1);

    border-radius: 4px 34px 4px 34px;

    background:
        rgba(255, 255, 255, 0.56);

    box-shadow:
        0 30px 70px rgba(107, 52, 62, 0.07);

    backdrop-filter: blur(12px);
}

.love-intro__letter::before {
    content: '';

    position: absolute;

    inset: 8px;

    border:
        1px solid rgba(169, 74, 90, 0.07);

    border-radius: 2px 28px 2px 28px;

    pointer-events: none;
}

.love-intro__quote {
    position: absolute;

    top: 8px;
    left: 24px;

    font-family: 'Cormorant Garamond', serif;

    font-size: 90px;
    font-style: italic;

    line-height: 1;

    color: rgba(169, 74, 90, 0.08);
}

.love-intro__letter p {
    position: relative;

    z-index: 2;

    font-family: 'Cormorant Garamond', serif;

    font-size: 19px;
    font-weight: 400;

    line-height: 1.8;

    color: var(--text-soft);
}

.love-intro__letter p+p {
    margin-top: 20px;
}

.love-intro__signature {
    position: relative;

    z-index: 2;

    display: block;

    margin-top: 30px;

    font-family: 'Cormorant Garamond', serif;

    font-size: 20px;
    font-style: italic;

    text-align: right;

    color: var(--red);
}

/* ============================= */
/* CONTINUAR */
/* ============================= */

.love-intro__continue {
    display: flex;
    flex-direction: column;
    align-items: center;

    gap: 12px;

    margin-top: 70px;
}

.love-intro__continue>span:first-child {
    font-family: 'Cormorant Garamond', serif;

    font-size: 17px;
    font-style: italic;

    color: rgba(107, 52, 62, 0.55);
}

.love-intro__arrow {
    font-size: 18px;

    color: var(--red);

    animation: arrowDown 1.8s ease-in-out infinite;
}

/* ============================= */
/* DECORACIONES */
/* ============================= */

.love-intro__decoration {
    position: absolute;

    color: rgba(169, 74, 90, 0.1);

    user-select: none;
    pointer-events: none;

    animation: float 7s ease-in-out infinite;
}

.love-intro__decoration--one {
    top: 15%;
    left: 8%;

    font-size: 80px;

    transform: rotate(-15deg);
}

.love-intro__decoration--two {
    top: 30%;
    right: 12%;

    font-size: 30px;

    animation-delay: -2s;
}

.love-intro__decoration--three {
    right: 7%;
    bottom: 15%;

    font-size: 100px;

    animation-delay: -4s;

    transform: rotate(20deg);
}

/* ============================= */
/* ANIMACIONES */
/* ============================= */

@keyframes heartbeat {

    0%,
    100% {
        transform: scale(1);
    }

    15% {
        transform: scale(1.15);
    }

    30% {
        transform: scale(1);
    }
}

@keyframes float {

    0%,
    100% {
        transform: translateY(0);
    }

    50% {
        transform: translateY(-16px);
    }
}

@keyframes arrowDown {

    0%,
    100% {
        opacity: 0.35;

        transform: translateY(0);
    }

    50% {
        opacity: 1;

        transform: translateY(8px);
    }
}

/* ============================= */
/* MOBILE */
/* ============================= */

@media (max-width: 600px) {
    .love-intro {
        padding:
            100px 20px;
    }

    .love-intro__letter {
        margin-top: 45px;

        padding:
            42px 25px;

        border-radius:
            3px 28px 3px 28px;
    }

    .love-intro__letter p {
        font-size: 17px;
    }

    .love-intro__decoration--one {
        left: -30px;
    }

    .love-intro__decoration--three {
        right: -40px;
    }
}
</style>