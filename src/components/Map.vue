<template>
  <div class="map">
    <h3>Карта офиса</h3>

    <div v-if="!isLoading" class="map-root">
      <MapSVG ref="svg" />
      <Table v-show="false" ref="table" />
    </div>
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import * as d3 from "d3";
import MapSVG from "../assets/images/map.svg";
import tables from "../assets/data/tables.json";
import Table from "../assets/images/workPlace.svg";
import legend from "../assets/data/legend.json";
import ClickOutside from "vue-click-outside"; // https://www.npmjs.com/package/vue-click-outside для клика скрытия профиля
import vClickOutside from "v-click-outside";
const { bind, unbind } = vClickOutside.directive;

export default {
  components: {
    MapSVG,
    Table,
  },
  data() {
    return {
      opened: false,
      isLoading: false,
      svg: null,
      g: null,
      tables: [],
      tableSvg: null,
    };
  },
  created() {
    this.tables = tables;
  },
  mounted() {
    this.isLoading = true;

    this.svg = d3.select(this.$refs.svg);
    this.g = this.svg.select("g");
    this.tableSvg = d3.select(this.$refs.table);

    if (this.g) {
      this.drawTables();
    } else {
      console.error("ERROR while drawing tables.");
    }
    this.isLoading = false;
  },
  methods: {
    drawTables() {
      const svgTablesGroupPlace = this.g
        .append("g")
        .classed("groupedTables", true);

      this.tables.map((table) => {
        const targetSeat = svgTablesGroupPlace
          .append("g")
          .attr("transform", `translate(${table.x}, ${table.y}) scale(0.5)`)
          .on("click", () => {
            d3.select(`#id_${table._id}`).classed("isShowingProfile", true);
            this.$emit("table-clicked", table._id);
            bind(document.querySelector(`#id_${table._id}`), {
              value: this.outsideClick,
            });
          })
          .attr("id", "id_" + table._id)
          .classed("employer-place", true);

        targetSeat
          .append("g")
          .attr("transform", `rotate(${table.rotate || 0})`)
          .attr("group_id", table.group_id)
          .classed("table", true)
          .html(this.tableSvg.html())
          .attr(
            "fill",
            legend.find((it) => it.group_id === table.group_id)?.color ??
              "transparent"
          );
      });
    },
    outsideClick() {
      const openedTable = d3.select(".isShowingProfile");
      if (openedTable) {
        const tableId = openedTable.attr("id");
        openedTable.classed("isShowingProfile", false);
        this.$emit("closed-profile", tableId);
        unbind(openedTable.node());
      }
    },
  },
  directives: {
    ClickOutside,
  },
};
</script>

<style scoped>
.map {
  height: 100%;
  width: 100%;
  padding: 24px;
  overflow: hidden;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}

.map-root {
  height: 100%;
  width: 100%;
  overflow: hidden;
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

::v-deep svg {
  height: 100%;
  width: 100%;
}

::v-deep .table {
  cursor: pointer;
}
</style>
