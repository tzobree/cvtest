<template>
  <div class="report-card market-prices">
    <div class="card-header">
      <div class="left">
        <h4 class="card-title">Market Prices</h4>
        <span class="tooltip">icon</span>
      </div>
      <div class="right">
        <span>arrow</span>
      </div>
    </div>
    <div class="card-content">

      <div class="grid">
        <div class="column">
          <p class="title">Dealer</p>
          <ul class="list">
            <li class="row">
              <p class="label">Label</p>
              <p class="value">$43024</p>
            </li>
            <li class="row good">
              <p class="label">Label</p>
              <p class="value">$43024</p>
            </li>
            <li class="row">
              <p class="label">Label</p>
              <p class="value">$43024</p>
            </li>
            <li class="row">
              <p class="label">Label</p>
              <p class="value">$43024</p>
            </li>
            <li class="row">
              <p class="label">Label</p>
              <p class="value">$43024</p>
            </li>
          </ul>
        </div>
        <div class="column">
          <p class="title">Dealer</p>
          <ul class="list">
            <li class="row">
              <p class="label">Label</p>
              <p class="value">$43024</p>
            </li>
            <li class="row good">
              <p class="label">Label</p>
              <p class="value">$43024</p>
            </li>
            <li class="row">
              <p class="label">Label</p>
              <p class="value">$43024</p>
            </li>
            <li class="row">
              <p class="label">Label</p>
              <p class="value">$43024</p>
            </li>
            <li class="row">
              <p class="label">Label</p>
              <p class="value">$43024</p>
            </li>
          </ul>
        </div>
      </div>
      
    </div>
  </div>
</template>

<style lang="scss" scoped>
.report-card.market-prices {
  border-radius: 0px 0px 20px 20px;
  .card-content {
    padding-top: 56px;
    .grid {
      .column {
        .title {
          line-height: 20px;
          background: #E0E9EC99;
          color: #062931;
          font-weight: 400;
          padding: 12px 10px;
          font-size: 0.875rem;
          margin: 0;
          border-radius: 5px;
        }
        .list {
          .row {
            &.good {
              .label, .value {
                color: #FB9100;
              }

            }
          }
        }
      }
    }
  }
}

</style>