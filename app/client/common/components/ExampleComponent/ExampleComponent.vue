<template>
    <div class="p-4 border border-dashed m-2 rounded">
        <ul v-if="characters?.length">
            <li v-for="character in characters" :key="character.id">
                {{ character.name }}
            </li>
        </ul>

        <div v-else>
            Loading...
        </div>
    </div>
</template>

<script lang="ts" setup>
    import { createHttpLink } from '@apollo/client/core'

    const httpLink = createHttpLink({
        uri: 'https://rickandmortyapi.com/graphql?api=getCharacters', 
    })

    const characters = ref()

    const query = gql`
        query {
            characters {
                results {
                    id
                    name
                }
            }
        }
    `

    const { clients } = useApollo()

    if (clients?.default) {
        clients.default.setLink(httpLink)
    }

    const loadUsers = async () => {
        const { data } = await useAsyncQuery(query)

        // @ts-ignore
        characters.value = data.value.characters.results
    }

    loadUsers()
</script>
