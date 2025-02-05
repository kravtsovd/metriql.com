# Google Data Studio

:::info
This integration is currently in beta and not verified by Google yet so it's not available in the marketplace.
:::

metriql has a community connector for Data Studio that converts Data Studio queries into metriql queries (segmentation), run the query on your data warehouse and let you visualize the data in Google Data Studio. In order to run the queries efficiently, the connector uses [segmentation](/query/segmentation) feature. 

#### Step 1:
<a href="https://datastudio.google.com/datasources/create?connectorId=AKfycbw8o0F6LEr0epNSNVWqNzlqo7R-6jRYxxSxBspzyg2Xi6SDFItLN_aM3l_U56Z0obwS" target="_blank">Connect your metriql on Google Data Studio</a>

#### Step 2:

You need to deploy metriql to a public environment and use the URL in GDS as follows:

<img src="/img/integrations/gds-login.png" alt="GDS Login Screen" style={{maxWidth: '600px'}}/>

1. Please add the `/api/v0` suffix to your metriql URL. If you're using metriql locally, you can use [a tunneling service](https://github.com/anderspitman/awesome-tunneling) to expose your local port to the internet and use the public URL in GDS.

2. If you don't have a password, you can skip the password input.


#### Step 3:

Once you connect to your metriql deployment, GDS connectors lists all the dataset available to the user as follows:

<img src="/img/integrations/gds-parameters.png" alt="GDS Parameter Screen" style={{maxWidth: '600px'}}/>

You should see the data model when you select the dataset. If you're using semantic types, you can define them in your YML files as follows:

```yml
columns:
    - name: price
      meta:
        metriql.dimension:
          report:
            datastudio:
              semantic_type: currency_usd
```

Please refer to the [GDS documentation](https://developers.google.com/apps-script/reference/data-studio/field-type) for all the available semantic types.

<img src="/img/integrations/gds-data-model.png" alt="GDS Data Model Screen" style={{maxWidth: '400px'}}/>

Congratulations, you're ready to create reports! 🎉

## Limitations

1. Since metriql doesn't support ad-hoc aggregations on dimensions, you should not drag & drop dimensions into metric as seen in the picture. GDS calculates the `sum(o_orderkey)` itself while `total_orders` is a measure defined in the metriql. If you have more than the maximum rows parameter, the value of `sum(o_orderkey)` won't be correct.

2. You can only use the fields in dimension & metric in the sorting. GDS doesn't pushdown this information to the connector so if you have a sorting field that is not being used as part of dimension & metric, metriql treats the sorting field as a dimension field.

<img src="/img/integrations/gds-report.png" alt="GDS Report" />
