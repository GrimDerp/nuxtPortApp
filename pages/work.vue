<template>
    <div class="bg-black h-full min-h-screen v-full px-8 py-12">
        <h2 class="text-white font-maven-bold text-7xl text-center opacity-0" 
            ref="title" 
            style="transform: translateY(30px)"> Work work work</h2>
           
        
    <div class="flex space-x-3">
        <div ref="projects" 
        v-for="project in projects" 
        class="w-full opacity-0"
        style="transform: translateX(30px)"
        >
        <nuxt-link to="/">
            <img 
            :src="project.image.url" 
            alt="fiber"
            style="height: 70vh;"
            class="object-cover"
            
             />
            <p class="text-white font-maven text-sm">{{ project.title }}</p>
        </nuxt-link>
        </div>
    </div>
</div>
</template>

<script>
import gsap from 'gsap'
import fiberImg from '~/assets/fiber.jpg'
import circuitImg from '~/assets/cBoard.jpg'
import planetImg from '~/assets/planet.jpg'
import matrixImg from '~/assets/matrix.jpg'
export default {
    data() {
        return {
            projects: [
                {
                    image: {
                      url: fiberImg
                    },
                    title: 'WEB'
                },
                {
                    image: {
                      url: circuitImg
                    },
                    title: 'HARDWARE'
                },
                {
                    image: {
                      url: fiberImg
                    },
                    title: 'WEB'
                },
                {
                    image: {
                      url: planetImg
                    },
                    title: 'MOBILE'
                },
                {
                    image: {
                      url: fiberImg
                    },
                    title: 'WEB'
                },
                {
                    image: {
                      url: fiberImg
                    },
                    title: 'WEB'
                },
                {
                    image: {
                      url: planetImg
                    },
                    title: 'WEB'
                },
                {
                    image: {
                      url: fiberImg
                    },
                    title: 'WEB'
                },
                {
                
                    image: {
                      url: fiberImg
                    },
                    title: 'WEB'
                },
                {
                    image: {
                      url: matrixImg
                    },
                    title: 'APPS'
                },
                {
                    image: {
                      url: fiberImg
                    },
                    title: 'WEB'
                }
            ]
        }
    },
    mounted() {
        gsap.to(this.$refs.title, {
            opacity: 1,
            duration: 4,
            y: 0,
            ease: 'expo'
        })
        gsap.to(this.$refs.projects, {
            opacity: 1,
            duration: 2,
            stagger: 0.1,
            x: 0,
            delay: 0.3 
        })
    }
}
</script>