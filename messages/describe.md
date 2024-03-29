---
description: Message to discover available resources in endpoints
---

# Describe Resources

This message enables discovering remote [resources ](../definitions.md#resources)defined both in client and server. Resources can be defined dynamically by clients and servers. As described in the [resources ](../definitions.md#resources)definition, a resource is basically like a function that can be executed like in RPC (Remote Procedure Call). As a "function" it can receive parameters, inputs, and provide an output.&#x20;

This message allows discovering such resources, like its names, function type, if they support streaming, required parameters, etc.

## Request

### Header

| Field            | Value                              | Description              |
| ---------------- | ---------------------------------- | ------------------------ |
| **Message Type** | 0x07                               | Describe Resources       |
| **Message Size** | [varint](../definitions.md#varint) | Remaining Message Length |

### Body

<table><thead><tr><th>Name</th><th>Field</th><th data-type="select">Type</th><th data-type="checkbox">Mandatory</th><th>Description</th></tr></thead><tbody><tr><td><strong>Stream Id</strong></td><td>0x01</td><td></td><td>true</td><td><a href="../definitions.md#stream-identifier">Stream identifier</a>.</td></tr><tr><td><strong>Parameters</strong></td><td>0x02</td><td></td><td>false</td><td>Params</td></tr><tr><td><strong>Resource</strong> </td><td>0x03</td><td></td><td>false</td><td><a href="../definitions.md#resource-definition">Resource identifier</a>. It should be a string or a resource identifier. If not sent, the target endpoint should report all available resources.</td></tr></tbody></table>

{% hint style="info" %}
If not resource is specified, the target endpoint should list all available resources.
{% endhint %}

## Response (All Resources)

If no resource is specified while sending the Describe Resources message, the target endpoint should list all available resources,

### Ok

If the request succeed, the remote endpoint should return an [Ok Message](ok.md) with the following payload:

```javascript
{
  "relay": {
    "fn": 2
    "id": 0
  },
  "temperature": {
    "fn": 3
    "id": 1
  },
  "reset" : {
    "fn": 1,
    "st": false,
    "id": 2
}
```

In such document each key represents the resource name

<table><thead><tr><th>Describe Field</th><th>Key</th><th>Value</th><th data-type="checkbox">Mandatory</th><th>Description</th></tr></thead><tbody><tr><td>Function Type</td><td><strong>fn</strong></td><td><p><strong>1</strong> : function without parameters</p><p><strong>2</strong> : function with input</p><p><strong>3</strong>: function with output</p><p><strong>4</strong>: function with input and output</p></td><td>true</td><td>Provides information about the function type, i.e., if it requires and input, provides an output, none, or both of them.</td></tr><tr><td>Parameters</td><td><strong>pr</strong></td><td><p><strong>true</strong>: requires parameters</p><p><strong>false</strong>: no parameters required</p></td><td>false</td><td>Determines if the resource requires parameters to be executed. By default, if not specified, the resource does not require parameters to be executed.</td></tr><tr><td>Stream</td><td><strong>st</strong></td><td><p><strong>true</strong>: support Stream</p><p><strong>false</strong>: no stream support</p></td><td>false</td><td>Determines if the resource can be used with the Streams functionality. By default, if not specified, the stream support is enabled.</td></tr><tr><td>Resource identifier</td><td><strong>id</strong></td><td>varint</td><td>false</td><td>An alternative numeric resource identifier, instead of a name, that can be used to call this resource. </td></tr></tbody></table>

### Error

The resources cannot be described, i.e., no support for describing resources.

## Response (Single Resource)

If a resource is specified while sending the Describe Resources message, the target endpoint should reply with the resource details.

### Ok

If the request succeed, the remote endpoint should return an [Ok Message](ok.md) with the following payload:

```json
{
  "in": true
}
```

```json
{
  "out": 22.33
}  
```

```json
{
  "in": 0
  "out": 25
}
```

### Error

The resource cannot be described, i.e, it does not exists.
