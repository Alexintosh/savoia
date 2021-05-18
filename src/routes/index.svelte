<script>
	import '../app.css';
    import Scale from '../portfolios/scale.json';
    import PortfolioCard from './../components/PortfolioCard.svelte';
    import { IndexCalculator } from '../classes/IndexCalculator';

    $: isLoading = true;
    let idx;

    async function main() {

        idx = new IndexCalculator('test', "0.2");
        await idx.pullData(false, Scale);
        idx.compute();
        console.log('idx', idx);
        isLoading = false;
    }

    main();
</script>

<main class="bg-gray-100 dark:bg-gray-800 relative h-screen overflow-hidden relative">
    
    <div class="bg-gray-900 flex justify-center items-center px-5 py-5">
        {#if isLoading == false}
            <PortfolioCard 
                startIndex={idx.performance[0][0]}
                name={idx.name}
                mCap={idx.cumulativeUnderlyingMCAP}
                author={"@alexintosh"}
                high={Math.max(...idx.nav.map( p => p[1]))}
                low={Math.min(...idx.nav.map( p => p[1]))}
                currentNAV={idx.nav[idx.nav.length-1][1]}
                startPrice={idx.indexStartingNAV}
                sharpeRatio={(idx.SHARPERATIO)}
                stDev={idx.STDEV}
                returns={`${(idx.performance[idx.performance.length-1][1] * 100).toFixed(2)}%`}
                chartData={{
                    labels: idx.nav.map( p => p[0]),
                    data: idx.nav.map( p => p[1]),
                }} 
            />
        {/if}
    </div>

</main>