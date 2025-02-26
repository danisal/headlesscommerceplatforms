<script lang="ts">
    import ValueWithNote from './ValueWithNote.svelte';
    import InfoButton from './InfoButton.svelte';
    import type { SolutionInfo } from '../types';
    import { camelToSentence, getLabel } from '$lib/utils';

    export let table: { data: SolutionInfo[] };

    const properties = Object.keys(table.data[0]).filter(
        (x) => !['name', 'website', 'type'].includes(x)
    );
</script>

<div class="flex flex-col text-sm pb-32">
    <div class="py-2 align-middle inline-block mx-auto">
        <div class="shadow border-b border-gray-200 p-1 bg-white sm:rounded-lg">
            <table
                class="relative flex flex-row flex-no-wrap divide-y divide-gray-200 bg-white bg-opacity-80"
            >
                <tbody class="divide-y divide-gray-200">
                    <tr class="hidden sm:table-row"
                        ><td
                            class="px-2 py-2 sm:px-5 sm:py-3 whitespace-nowrap sticky left-0 bg-gray-100"
                            >Solution</td
                        >
                        {#each table.data as solution}
                            <td
                                class="sticky top-0 px-2 py-2 sm:px-5 sm:py-3 bg-gradient-to-b from-white via-white to-transparent bg-opacity-70 whitespace-nowrap text-lg"
                                >{solution.name}</td
                            >
                        {/each}
                    </tr>
                    <tr class="flex flex-col sm:table-row"
                        ><td class="px-2 py-2 sm:px-5 sm:py-3 whitespace-nowrap sticky left-0 bg-gray-100"
                            >Website</td
                        >
                        {#each table.data as solution}
                            <td class="px-2 py-2 sm:px-5 sm:py-3 whitespace-nowrap">
                                <span class="sm:hidden text-sm">{solution.name}:</span>
                                <a class="text-blue-600 hover:text-blue-500" href={solution.website}
                                    >{solution.website}</a
                                >
                            </td>
                        {/each}
                    </tr>
                    {#each properties as prop}
                        <tr class="flex flex-col sm:table-row group">
                            <td
                                class="px-2 py-2 sm:px-5 sm:py-3 whitespace-nowrap group-hover:bg-indigo-50 transition-colors sticky left-0 bg-gray-100"
                            >
                                <div class="flex space-x-2">
                                    <span>{camelToSentence(prop)}</span>
                                    <InfoButton label={getLabel(prop)} />
                                </div>
                            </td>
                            {#each table.data as solution}
                                <td
                                    class="px-2 py-2 sm:px-5 sm:py-3 flex sm:table-cell whitespace-nowrap group-hover:bg-indigo-50 transition-colors"
                                >
                                    <span class="sm:hidden text-sm mr-2">{solution.name}:</span>
                                    {#if solution[prop]}
                                        <ValueWithNote
                                            item={solution[prop]}
                                            solutionName={solution.name}
                                            feature={prop}
                                        />
                                    {/if}
                                </td>
                            {/each}
                        </tr>
                    {/each}
                </tbody>
            </table>
        </div>
    </div>
</div>
