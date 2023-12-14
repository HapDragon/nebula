<template>
    <div id="app">
        <!--<network :nodeList="nodes" :linkList="links"></network>-->
        <network
                :nodeList="nodes"
                :linkList="links"
                :nodeSize="nodeSize"
                :linkWidth="linkWidth"
                :linkDistance="linkDistance"
                :svgTheme="svgTheme?'dark':'light'"
                :bodyStrength="bodyStrength"
        ></network>
    </div>
</template>

<script>
	import Network from "./Network.vue";
	import axios from "axios";

	axios.defaults.headers["Content-Type"] = "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7";
	axios.defaults.headers["Access-Control-Allow-Origin"] = "*";
	axios.defaults.withCredentials = true;

	export default {
		name: "norel",
		components: {
			Network
		},
		data() {
			return {
				// nodes: [],
				// links: [],
				showSettingCard: false,
				nodeSize: 14,
				linkWidth: 2,
				linkDistance: 220,
				bodyStrength: -205,
				svgTheme: false, // light
				nodes: [

				],
				links: [

				]
			};
		},
		mounted() {
			const url='/example.json';
			const scope=this;
			axios.get(url)
				.then((response) =>{
					scope.nodes=response.data.nodes;

					let links=response.data.links.map((item,index)=>{
						item.id=index.toString();item.value=item.name;item.index=item.id;
						return item;
					// scope.links=response.data.links.map(item=>{item.label=item.name;return item;
                        });


					const linksourcetarget = new Map();

					links.forEach(item=>{
						const id=item.source+"___to___"+item.target;
						const value=linksourcetarget.get(id);
						if(value==null){
							linksourcetarget.set(id,1);
							item.linkranknum=1;
						}
						else{

							let n=Number(value);
							n+=1;
							item.linkranknum=n;

							linksourcetarget.set(id,n);

						}

					})

                    scope.links=links;

					debugger

                });
		}
	};
</script>

<style>
    body {
        margin: 0;
    }
</style>

