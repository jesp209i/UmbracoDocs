---
description: Key Umbraco node properties used by Umbraco Commerce.
---

# Umbraco Properties

Umbraco Commerce uses Umbraco nodes as its source of information. In order for Umbraco Commerce to gather the information it needs, however, it requires that a number of properties are defined at different locations with specific property aliases.

## Properties

<table>
  <thead>
    <tr>
      <th width="189.89867841409693">Alias</th>
      <th width="200">Type</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>store</code></td>
      <td>Umbraco.Commerce.StorePicker</td>
      <td>
        Often placed on the site root node, but can be placed on any node higher
        than the product nodes themselves, this property links the website to a
        specific Umbraco Commerce store configuration.
      </td>
    </tr>
    <tr>
      <td><code>productName</code></td>
      <td>Textstring</td>
      <td>
        Optional product node property that allows you to define an explicit
        product name other than the product nodes <code>.Name</code> property,
        which will be used as fallback.
      </td>
    </tr>
    <tr>
      <td><code>sku</code></td>
      <td>Textstring</td>
      <td>
        Product node property defining the unique <code>SKU</code> of the
        product.
      </td>
    </tr>
    <tr>
      <td><code>price</code></td>
      <td>Umbraco.Commerce.Price</td>
      <td>Product node property defining the prices for the product.</td>
    </tr>
    <tr>
      <td><code>stock</code></td>
      <td>Umbraco.Commerce.Stock</td>
      <td>Product node property defining the stock level of the product.</td>
    </tr>
    <tr>
      <td><code>image</code></td>
      <td>Umbraco.MediaPicker3</td>
      <td>Optional product node property defining an image for the given product.</td>
    </tr>
    <tr>
      <td><code>measurements</code></td>
      <td>Umbraco.Commerce.Measurements</td>
      <td>Optional (depending on the shipping configuration) node property defining physical dimensions and weight of the product.</td>
    </tr>
    <tr>
      <td><code>taxClass</code></td>
      <td>Umbraco.Commerce.StoreEntityPicker</td>
      <td>
        Optional product node property that allows you to define an explicit
        <code>Tax Class</code> for the product, should it differ from the stores
        default.
      </td>
    </tr>
    <tr>
      <td><code>isGiftCard</code></td>
      <td>True/False</td>
      <td>
        Optional product node property that defined whether the product node
        should be considered a Gift Card product, in which case it triggers the
        automatic generation of a Gift Card in the backoffice and emails it
        directly to the customer on checkout.
      </td>
    </tr>
    <tr>
      <td><code>productSource</code></td>
      <td>ContentPicker</td>
      <td>
        Optional product node property allowing you to link a product to another
        product outside of it's hierarchy to be used as it's source of product
        information.
      </td>
    </tr>
  </tbody>
</table>

## Property Editors

Umbraco Commerce includes default property editors that help manage and configure eCommerce functionalities within the Umbraco backoffice.

![Built-in Property Editors](images/property-editors.png)

The available property editors include:

* **Price:** Used to manage and define product pricing.
* **Store Picker:** Allows selection of a specific store for products or configurations.
* **Store Entity Picker:** Used for selecting store entities such as currencies, locations and payment / shipping methods.
* **Stock:** Helps manage stock levels for products.
* **Measurements:** Allows the configuration of product dimensions and weight.
* **Variants Editor:** Used for managing [complex product variants](product-variants/complex-variants.md), such as sizes or colors.
