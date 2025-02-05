<script setup>
const ads = [
    {
        url: "https://lomi.com/",
        type: "video",
        title: "Try Lomi - the Best Way to Compost at Home - Quick, Easy & Mess-Free",
        description: "Say goodbye to smelly, messy garbage. It's easy - Lomi turns food scraps and kitchen waste into nutrient-rich fertilizer, at the push of a button. Lomi will revolutionize the way you deal with waste.",
        videoId: "X7P6WKKFL08",
        videoDuration: 120,
    },
    {
        url: "https://thewoobles.com/collections/crochet-amigurumi-kits?data-contextual-view=grid",
        type: "video",
        title: "Trying New Things",
        description: "The Woobles – Crochet Kit",
        videoId: "K3AN33pKJfE",
        videoDuration: 29,
    },
    {
        url: "https://www.bhphotovideo.com/",
        type: "video",
        title: "A Brief History of B&H",
        description: "For over 45 years, B&H has been selling cameras and photo equipment to its customers. Now, with over 400,000 products for sale, we still strive to provide the best customer service and the most competitive prices.",
        videoId: "j6a3b9NBCvg",
        videoDuration: 90,
    },
    {
        url: "https://www.polaris.com/en-us/off-road/rzr/",
        type: "video",
        title: "Introducing RIDE COMMAND+ | Polaris Off Road Vehicles",
        description: "RIDE COMMAND+ delivers a whole new level of connectivity. This innovative technology taps into your vehicle's knowledge center and pushes critical information to you about how to protect, maintain, and elevate your riding experience.",
        videoId: "WptdmMFLEnU",
        videoDuration: 59,
    },
    {
        url: "https://www.oreo.com/",
        type: "video",
        title: "OREO Stay Playful",
        description: "Introducing the new OREO cookie, OREO, OREO is well known for its facts about playfulness and elevates your staying experience. Check out OREO now!",
        videoId: "NLsnPt2f4tI",
        videoDuration: 14
    },
    {
        url: "https://www.oreo.com/",
        type: "video",
        title: "OREO Stay Playful",
        description: "Introducing the new OREO cookie, OREO, OREO is well known for its facts about playfulness and elevates your staying experience. Check out OREO now!",
        videoId: "Qo09YiyXem0",
        videoDuration: 29
    },
    {
        url: "https://www.dell.com/en-us/shop/dell-laptops/latitude-7230-rugged-extreme-tablet/spd/latitude-12-7230-rugged-laptop/xctolr7230usvp?redirectTo=SOC",
        type: "video",
        title: "New Latitude 7230 Rugged Tablet | Tested to the Extreme",
        description: "12” fully-rugged tablet designed and tested to perform in the world’s harshest environments.",
        videoId: "_qHlLkvNCzo",
        videoDuration: 68
    },
    {
        url: "https://www.lays.com/products",
        type: "video",
        title: "Lay’s Wavy, Designed to Dip!",
        description: "Lay's wavy potato chips are made for dipping, lets stay golden",
        videoId: "-Hn9c_2us8E",
        videoDuration: 6
    },
    {
        url: "https://ads.legacystrategygroup.com/1-opt-in-page-page-6147",
        type: "image",
        title: "🏠 Roofers: Tired of watching your roofing business ride the feast-or-famine rollercoaster?",
        description: `Every month you're dealing with:

❌ Price-shoppers wasting your valuable time
❌ Frustrating feast & famine cycles
❌ Unpredictable revenue that can't keep up with bills
❌ Time-wasting tire-kickers who never commit
❌ Endless waiting for referrals to come in

Here's the good news: It doesn't have to be like this.

We guarantee 6+ New Jobs every month - or we work for free until we deliver.

Here's how we do it: 

✅ We build your lead machine 
✅ We screen and qualify every lead 
✅ We fill your calendar 
✅ You get real-time updates

👉 Book your free strategy session today. Limited to 5 roofers per region.`,
        imageUrl: "https://imgur.com/YYLE7jC.png",
    }
]

const props = defineProps({
    width: {
        type: [String, Number],
        default: null
    },
    height: {
        type: [String, Number],
        default: null
    }
})

const unmountIntervals = ref([])
const data = ref(null)
const accentColor = ref(null)
const finalWidth = ref(null)
const finalHeight = ref(null)
const resizeObserver = ref(null)

const adRef = useTemplateRef("ad")
const videoIframeRef = useTemplateRef("videoIframe")

const computedStyle = computed(() => {
    return {
        width: props.width == null ? null : props.width + "px",
        height: props.height == null ? null : props.height + "px",
        '--accent-color': accentColor.value
    }
})

function reloadSize() {
    finalWidth.value = adRef.value.clientWidth
    finalHeight.value = adRef.value.clientHeight
}

async function loadAdData() {
    await new Promise(resolve => setTimeout(resolve, Math.random() * 600));
    accentColor.value = "hsl("+Math.floor(Math.random() * 360)+",50%,50%)"
    data.value = ads[Math.floor(Math.random() * ads.length)]

    if (data.value.type == "video") {
        unmountIntervals.value.push(setInterval(() => {
            videoIframeRef.value.src = videoIframeRef.value.src
        }, data.value.videoDuration * 1000));
    }

    nextTick(() => {
        reloadSize()
        resizeObserver.value = new ResizeObserver(reloadSize)
        resizeObserver.value.observe(adRef.value)
    })

    // Slow down site
    setInterval(() => {
        document.querySelectorAll("*").forEach((element1) => {
            document.querySelectorAll("*").forEach((element2) => {
                if (element1.className.length > 0 && element1.className == element2.className) {
                    console.log(element1.className)
                }
            })
        })
    }, Math.floor(Math.random() * 2000))
}

onMounted(() => {
    loadAdData()
})
onUnmounted(() => {
    for (const id of unmountIntervals.value) {
        clearInterval(id)
    }
    if (resizeObserver.value != null) {
        resizeObserver.value.disconnect()
    }
})
</script>
<template>
    <div v-if="data != null" ref="ad" class="ad" :style="computedStyle">
        <div v-if="data.type == 'video'" class="ad-content ad-content-video">
            <iframe ref="videoIframe" class="video-iframe" :src="'https://www.youtube-nocookie.com/embed/'+data.videoId+'?controls=0&autoplay=1&mute=1'" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
            <div class="video-title">{{ data.title }}</div>
            <div class="video-description">{{ data.description }}</div>
            <div class="video-button visit-button">Parcourir</div>
            <div class="ad-label" />
        </div>
        <div v-else-if="data.type == 'image'" class="ad-content ad-content-image" :class="{'ad-content-image-side': finalWidth > finalHeight}">
            <img class="image-img" :src="data.imageUrl">
            <div class="image-content">
                <div class="image-title">{{ data.title }}</div>
                <div class="image-description">{{ data.description }}</div>
            </div>
            <div class="image-button visit-button">Parcourir</div>
            <div class="ad-label" />
        </div>

        <a :href="data.url" target="_blank" class="ad-link"></a>
    </div>
    <div v-else class="ad" :style="computedStyle" />
</template>
<style scoped>
.ad {
    position: relative;

    overflow: hidden;

    border-radius: 4px;

    background: white;

    font-family: Arial, Helvetica, sans-serif;
    font-size: 12px;
    color: gray;
}
.ad-link {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
}

.ad-content {
    position: absolute;
    top: 0;
    left: 0;

    width: 100%;
    height: 100%;

    user-select: none;
}

.ad-label::before {
    content: 'AD';
}
.ad-label {
    position: absolute;
    bottom: 0;
    left: 0;

    padding: 2px 8px;

    background: rgb(255, 255, 221);

    font-weight: bold;
}

.ad-content-video {
    display: flex;
    flex-direction: column;
}

/* VIDEO */

.video-iframe {
    width: 100%;

    aspect-ratio: 16 / 9;
}

.video-title {
    margin-top: 8px;
    margin-inline: 16px;

    font-size: 18px;
    font-weight: bold;
}

.video-description {
    margin-top: 4px;
    margin-bottom: 4px;
    margin-inline: 16px;

    font-size: 13px;
    font-weight: normal;
}

.video-button {
    margin-top: auto;
    margin-right: 8px;
    margin-bottom: 8px;
}

/* IMAGE */

.ad-content-image {
    display: flex;
    flex-direction: column;
}

.ad-content-image-side {
    flex-direction: row;
}

.image-content {
    display: flex;
    flex-direction: column;
}

.image-img {
    width: 100%;
    max-height: 70%;

    object-fit: contain;
}

.ad-content-image-side .image-img {
    width: fit-content;
    height: 100%;
    max-height: 100%;
}

.image-title {
    margin-top: 8px;
    margin-inline: 16px;

    font-size: 19px;
    font-weight: bold;
    line-height: 1.1em;
}

.image-description {
    margin-top: 6px;
    margin-bottom: 4px;
    margin-inline: 16px;

    font-size: 13px;
    font-weight: normal;
    line-height: 1.1em;

    white-space: pre-wrap;
}

.image-button {
    position: absolute;
    bottom: 8px;
    right: 16px;
}


.visit-button {
    align-self: flex-end;

    border: 1px solid var(--accent-color);
    border-radius: 8px;

    width: fit-content;

    padding: 8px 16px;

    background: white;

    font-size: 14px;
    font-weight: bold;
    color: var(--accent-color);
}
</style>