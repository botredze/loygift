<template>
  <div class="parent-modal">
    <div class="modal myModal fade" id="history-promocode" tabindex="-1" role="dialog" aria-labelledby="history-promocode" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content category-content">
          <div class="modal-header category-header">
            <div class="d-flex justify-content-between mb-3" style="width:100%">
              <h3 class="modal-title">Promocode - <span id="promoHistoryTitle"></span></h3>
              <button type="button" data-dismiss="modal" aria-label="Close" class="close">
              <span aria-hidden="true">
                <img src="../../assets/icons/x.svg" alt="">
              </span>
              </button>
            </div>

          </div>

          <div class="modal-body category-body">

            <div class="d-flex main-content-header">
              <div class="table-head" style="width: 50%;">Name</div>
              <div class="table-head" style="width: 40%;">Date used</div>
              <div class="table-head" style="width:10%">Used</div>
            </div>
            <div class="table-content promoHistoryContent">
            </div>

          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HistoryPromocode"
}
</script>

<style scoped>
.category-content{
  border:0;
  border-radius: 0;
  width: 643px;
  height: 782px;
  margin-top: 8%;
}
.modal-title{
  color:#222;
}
.main-content-header{
  padding-left:35px;
  padding-right: 35px;
}
.modal-header{
  padding: 27px 20px;

}
</style>