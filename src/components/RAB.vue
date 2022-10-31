<template>
  <div>
    <button class="d-print-none" @click="handlePrint()">Print</button>
    <br />
    <div class="paper">
      <div class="container">
        <div class="row">
          <div class="col-8">
            <img
              src="https://i.ibb.co/rQ3KcqN/logo-1.png"
              alt="logo-1"
              border="0"
              width="150px"
            />
            <div
              style="
                margin-top: 10px;
                color: #343343;
                font-family: 'Segoe UI';
                font-weight: 600;
                font-size: 12px;
              "
            >
              <p style="margin: 0; font-size: 14px; font-weight: 800">
                Melayani: Perbaikan Pompa Air, Kelistrikan, Pasang Water Filter,
                Jual-Beli Water Filter.
              </p>
              <p style="margin: 0">
                JL. TUNJUNG BIRU III, NO. 17A Denpasar Bali 80223
              </p>
              <p style="margin: 0">Telp. 0823-4249-4162</p>
            </div>
          </div>
          <div class="col-4">
            <div
              style="
                color: #343343;
                font-family: 'Segoe UI';
                font-weight: 600;
                font-size: 12px;
                background: #fff;
                padding: 10px 20px;
                border-radius: 6px;
              "
            >
              <p class="text-center" style="margin-bottom: 2px">
                Denpasar {{ this.today }}
              </p>
              <p class="text-center">
                Kepada Yth, <br />
                <textarea type="text" class="text-center form-control" />
              </p>
            </div>
          </div>
        </div>
      </div>

      <span class="separator"></span>

      <h1 style="font-size: 24px" class="text-center">
        Rancangan Anggaran Biaya (RAB)
      </h1>

      <br />
      <table class="table table-bordered">
        <thead>
          <tr>
            <td width="30">No</td>
            <td>Keterangan</td>
            <td>Harga</td>
            <td width="50">Tidakan</td>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(row, index) in rows" :key="index">
            <td width="50">{{ index + 1 }}</td>
            <td>
              <textarea
                class="form-control hide-border"
                type="keterangan"
                v-model="row.keterangan"
              />
            </td>
            <td>
              <input
                class="form-control hide-border"
                type="number"
                v-model="row.harga"
              />
            </td>
            <td>
              <button
                class="d-print-none btn btn-danger"
                @click="deleteRow(index)"
              >
                Hapus
              </button>
            </td>
          </tr>
          <tr>
            <td></td>
            <td>
              <p class="text-end"><b>Total</b></p>
            </td>
            <td>
              <p>
                {{ totalFormatted }}
              </p>
            </td>
            <td></td>
          </tr>
        </tbody>
        <tfoot>
          <tr>
            <td colspan="4" class="text-center">
              <button
                class="btn d-print-none btn-small btn-success"
                @click="addRow()"
              >
                Tambah
              </button>
            </td>
          </tr>
        </tfoot>
      </table>

      <div class="flex">
        <div class="tanda-tangan hide-border">
          <!-- <h2>Kontraktor</h2> -->
          <!-- <h3>Samsul Hadi</h3> -->
        </div>
        <div class="tanda-tangan hide-border">
          <h2>Pelaksana Proyek</h2>
          <h3>Samsul Hadi</h3>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RAB',
  data() {
    return {
      client: {
        nama: 'Buk Agung',
        lokasi: 'Jalan Batang Hari, Bali Denpasar',
      },
      rows: [
        {
          keterangan: 'doing 1',
          harga: 4000,
        },
        {
          keterangan: 'doing 2',
          harga: 6000,
        },
      ],
      total: 0,
    };
  },
  mounted() {
    this.$watch(
      'rows',
      function (row) {
        this.hitungTotal();
      },
      { deep: true }
    );

    this.hitungTotal();
  },
  computed: {
    today() {
      let d = new Date();
      let str = '';
      str +=
        String(d.getDate()) +
        '-' +
        String(d.getMonth() + 1) +
        '-' +
        d.getFullYear();
      return str;
    },
    totalFormatted() {
      return this.priceFormat(this.total);
    },
  },
  methods: {
    handlePrint() {
      print();
    },
    priceFormat(price) {
      return 'Rp. ' + Intl.NumberFormat('id').format(price);
    },
    hitungTotal() {
      let harga = 0;
      this.rows.forEach((element) => {
        harga += Number(element.harga);
      });
      this.total = harga;
    },
    addRow() {
      this.rows.push({
        keterangan: 'Doing-' + (this.rows.length + 1),
        harga: 0,
      });
    },
    deleteRow(index) {
      this.rows.splice(index, 1);
    },
  },
};
</script>

<style>
* {
  font-family: 'Segoe UI', sans-serif;
}
.separator {
  display: block;
  border: dashed 2px #343343;
  margin: 10px 0;
}
.paper {
  width: 960px;
  padding: 10px;
  margin: auto;
}
.flex {
  display: flex;
  justify-content: space-between;
}
.tanda-tangan {
  width: 300px;
  border: 1px solid #ccc;
  min-height: 100px;
  position: relative;
}

.tanda-tangan h2 {
  font-size: 14px;
  position: absolute;
  top: 5px;
  left: 0;
  right: 0;
  font-weight: 700;
}

.tanda-tangan h3 {
  font-weight: 600;
  font-size: 12px;
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  text-decoration: underline;
}

@media print {
  .hide-print {
    display: none;
  }
  .hide-border {
    border: none !important;
  }
}
</style>
