<script>
	import '../app.css';
    import portfolios from '../portfolios/index';
    import PortfolioCard from './../components/PortfolioCard.svelte';
    import { IndexCalculator } from '../classes/IndexCalculator';

    $: isLoading = true;
    async function main() {
        for (let i = 0; i < portfolios.length; i++) {
            const p = portfolios[i];
            p.model = new IndexCalculator(p.name);
            await p.model.pullData(false, p.json);
            p.model.computeAll({
                adjustedWeight: false,
                sentimentWeight: false,
                computeWeights: false
            });
            console.log('idx', p);
            console.log('idx', JSON.stringify(p));
            p.ready = true;
        }
        isLoading = false;
    }

    main();
</script>

<main class="bg-gray-100 dark:bg-gray-800 relative h-screen overflow-hidden relative">
    
    <div class="bg-gray-900 flex justify-center items-center px-5 py-5">
        {#each portfolios as idx }
            <div class="w-full">
            {#if isLoading === false}    
                <PortfolioCard 
                    startIndex={idx.model.performance[0][0]}
                    name={idx.name}
                    mCap={idx.model.cumulativeUnderlyingMCAP}
                    author={idx.author}
                    high={Math.max(...idx.model.nav.map( p => p[1]))}
                    low={Math.min(...idx.model.nav.map( p => p[1]))}
                    currentNAV={idx.model.nav[idx.model.nav.length-1][1]}
                    startPrice={idx.model.indexStartingNAV}
                    sharpeRatio={(idx.model.SHARPERATIO)}
                    stDev={idx.model.STDEV}
                    returns={`${(idx.model.performance[idx.model.performance.length-1][1] * 100).toFixed(2)}%`}
                    chartData={{
                        labels: idx.model.nav.map( p => p[0]),
                        data: idx.model.nav.map( p => p[1]),
                    }} 
                />
            {:else}
                Loading ⏲
            {/if}
            </div>
        {/each}
        
    </div>

</main>