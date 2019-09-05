 <template>
 <div>
 <script type="text/x-template" id="polygraph-template">
  <g>
    <polygon :points="points"></polygon>
    <circle cx="100" cy="100" r="80"></circle>
    <axis-label
      v-for="(stat, index) in stats"
      :stat="stat"
      :index="index"
      :key="stat.id"
      :total="stats.length">
    </axis-label>
  </g>
    </script>

    <!-- template for the axis label component. -->
    <script type="text/x-template" id="axis-label-template">
        <text :x="point.x" :y="point.y">{{stat.label}}</text>
    </script>
    </div>
</template>
<script>
import Vue from "vue";
// A resusable polygon graph component
Vue.component("polygraph", {
  props: ["stats"],
  
  computed: {
    // a computed property for the polygon's points
    points: function() {
      var total = this.stats.length;
      return this.stats
        .map(function(stat, i) {
          var point = valueToPoint(stat.value, i, total);
          return point.x + "," + point.y;
        })
        .join(" ");
    }
  },
  components: {
    // a sub component for the labels
    "axis-label": {
      props: {
        stat: Object,
        index: Number,
        total: Number
      },
      template: "#axis-label-template",
      computed: {
        point: function() {
          return valueToPoint(+this.stat.value + 10, this.index, this.total);
        }
      }
    }
  }
});

// math helper...
function valueToPoint(value, index, total) {
  var x = 0;
  var y = -value * 0.8;
  var angle = ((Math.PI * 2) / total) * index;
  var cos = Math.cos(angle);
  var sin = Math.sin(angle);
  var tx = x * cos - y * sin + 100;
  var ty = x * sin + y * cos + 100;
  return {
    x: tx,
    y: ty
  };
}
</script>