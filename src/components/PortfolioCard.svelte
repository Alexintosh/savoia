<script>
    import { onMount } from 'svelte';
    import moment from 'moment';
    export let chartData, currentNAV, sharpeRatio, stDev, returns, name, author, startDate, startPrice, high, low, mCap;
    

    const renderChart = (chartData) => {
        let c = false;

        Chart.helpers.each(Chart.instances, function(instance) {
            if (instance.chart.canvas.id == `chart-${name}`) {
                c = instance;
            }
        });

        if(c) {
            c.destroy();
        }

        let ctx = document.getElementById(`chart-${name}`).getContext('2d');

        let chart = new Chart(ctx, {
            type: "line",
            data: {
                labels: chartData.labels,
                datasets: [
                    {
                        label: '',
                        backgroundColor: "rgba(255, 255, 255, 0.1)",
                        borderColor: "rgba(255, 255, 255, 1)",
                        pointBackgroundColor: "rgba(255, 255, 255, 1)",
                        data: chartData.data,
                    },
                ],
            },
            layout: {
                padding: {
                    right: 10
                }
            },
            options: {
                legend: {
                    display: false,
                },
                scales: {
                    yAxes: [{
                        ticks: {
                            fontColor: "rgba(255, 255, 255, 1)",
                        },
                        gridLines: {
                            display: false,
                        },
                    }],
                    xAxes: [{
                        ticks: {
                            fontColor: "rgba(255, 255, 255, 1)",
                        },
                        gridLines: {
                            color: "rgba(255, 255, 255, .2)",
                            borderDash: [5, 5],
                            zeroLineColor: "rgba(255, 255, 255, .2)",
                            zeroLineBorderDash: [5, 5]
                        },
                    }]
                }
            }
        });
    }

    onMount(async () => {
		renderChart(chartData)
	});

</script>

<div class="rounded shadow-xl overflow-hidden w-full md:flex" style="max-width:900px">
    <div class="flex w-full md:w-1/2 px-5 pb-4 pt-8 bg-indigo-500 text-white items-center">
        <canvas id={`chart-${name}`} class="w-full"></canvas>
    </div>
    <div class="flex w-full md:w-1/2 p-10 bg-gray-100 text-gray-600 items-center">
        <div class="w-full">
            <h3 class="text-lg font-semibold leading-tight text-gray-800">{name}</h3>
            <h6 class="text-sm leading-tight mb-2">
                <span>{author}</span>&nbsp;&nbsp;-&nbsp;&nbsp; Start: {moment(startDate).format("MMM Do YY")}</h6>
            <div class="flex w-full items-end mb-6">
                <span class="block leading-none text-3xl text-gray-800">${currentNAV.toFixed(2)}</span>
                <span class="block leading-5 text-sm ml-4 text-green-500">▼ ▲ {returns}</span>
            </div>
            <div class="flex w-full text-xs">
                <div class="flex w-5/12">
                    <div class="flex-1 pr-3 text-left font-semibold">High</div>
                    <div class="flex-1 px-3 text-right">{high.toFixed(2)} $</div>
                </div>
                <div class="flex w-7/12">
                    <div class="flex-1 px-3 text-left font-semibold">Market Cap</div>
                    <div class="flex-1 pl-3 text-right">{mCap.toFixed(0)} $</div>
                </div>
            </div>
            <div class="flex w-full text-xs">
                <div class="flex w-5/12">
                    <div class="flex-1 pr-3 text-left font-semibold">Low</div>
                    <div class="px-3 text-right">{low.toFixed(2)} $</div>
                </div>
                <div class="flex w-7/12">
                    <div class="flex-1 px-3 text-left font-semibold">Sharpe</div>
                    <div class="pl-3 text-right">{sharpeRatio.toFixed(2)}</div>
                </div>
            </div>
            <div class="flex w-full text-xs">
                <div class="flex w-5/12">
                    <div class="flex-1 pr-3 text-left font-semibold">Start</div>
                    <div class="px-3 text-right">{startPrice} $</div>
                </div>
                <div class="flex w-7/12">
                    <div class="flex-1 px-3 text-left font-semibold">St Dev</div>
                    <div class="pl-3 text-right">{stDev.toFixed(2)}</div>
                </div>
            </div>
        </div>
    </div>
</div>