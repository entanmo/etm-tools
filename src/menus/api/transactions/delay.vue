<template>
  <div class="wrapper-content">
    <secret :modules="modules"
            :item="item" />
    <amount :modules="modules"
            :item="item" />
    <recipientId :modules="modules"
                 :item="item" />
    <publicKey :modules="modules"
               :item="item" />
    <secondSecret :modules="modules"
                  :item="item" />
    <multisigAccountPublicKey :modules="modules"
                              :item="item" />
    <message :modules="modules"
             :item="item" />
    <args :modules="modules"
          :item="item" />
  </div>
</template>

<script>
import secret from "@/components/widgets/secret.vue";
import amount from "@/components/widgets/amount.vue";
import recipientId from "@/components/widgets/recipientId.vue";
import publicKey from "@/components/widgets/publicKey.vue";
import secondSecret from "@/components/widgets/secondSecret.vue";
import multisigAccountPublicKey from "@/components/widgets/multisigAccountPublicKey.vue";
import message from "@/components/widgets/message.vue";
import args from "@/components/widgets/args.vue";

export default {
  data() {
    return {
      modules: "transactions",
      item: "delay"
    };
  },
  components: {
    secret,
    amount,
    recipientId,
    publicKey,
    secondSecret,
    multisigAccountPublicKey,
    message,
    args
  }
};
</script>

<style lang="less" scoped>
</style>
