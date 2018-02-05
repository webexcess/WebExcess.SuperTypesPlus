# WebExcess.SuperTypesPlus
[![Latest Stable Version](https://poser.pugx.org/webexcess/supertypes-plus/v/stable)](https://packagist.org/packages/webexcess/supertypes-plus)
[![License](https://poser.pugx.org/webexcess/supertypes-plus/license)](https://packagist.org/packages/webexcess/supertypes-plus)

The Neos SuperTypes allow you to combine multiple sets of NodeType-Definitions, so called Mixins. That's awesome, until you get overlapping property names.

With this package you become the possibility to modify Mixins for each SuperType-Definition.

## Installation

    composer require webexcess/supertypes-plus

## Example

Mixin Source:
```yaml
'Vendor.Package:ImageMixin':
  abstract: true
  ui:
    inspector:
      groups:
        image:
          label: i18n
          icon: icon-picture
  properties:
    image:
      type: Neos\Media\Domain\Model\ImageInterface
      ui:
        label: i18n
        inspector:
          group: image
          position: 100
    caption:
      type: string
      ui:
        label: i18n
        inspector:
          group: image
          position: 200
```

NodeType Usage:
```yaml
'Vendor.Package:ImageCollage':
  superTypes:
    'Neos.Neos:Content': true
    'Vendor.Package:ImageMixin':
      -
        properties:
          '*': 'left*'
      -
        ui: false
        properties:
          '*': 'right*'
  ui:
    label: i18n
    icon: icon-picture
  properties:
    rightImage:
      ui:
        inspector:
          position: 300
    rightCaption:
      ui:
        inspector:
          position: 400
```

Generated Result:
```yaml
'Vendor.Package:ImageCollage':
  superTypes:
    'Neos.Neos:Content': true
    'Vendor.Package:ImageMixin-9073ec087c07c8365a776639b521c38a6688dbf7': true
    'Vendor.Package:ImageMixin-f6dc591593ef2c992ef29315ad58bf07a2b565a5': true
  ui:
    label: i18n
    icon: icon-picture
  properties:
    rightImage:
      ui:
        inspector:
          position: 300
    rightCaption:
      ui:
        inspector:
          position: 400
  
# Autogenerated Mixin of the first SuperType-Modification..
'Vendor.Package:ImageMixin-9073ec087c07c8365a776639b521c38a6688dbf7':
  abstract: true
  ui:
    inspector:
      groups:
        image:
          label: 'Vendor.Package:NodeTypes.ImageMixin:groups.image'
          icon: icon-picture
  properties:
    leftImage:
      type: Neos\Media\Domain\Model\ImageInterface
      ui:
        label: 'Vendor.Package:NodeTypes.ImageMixin:properties.image'
        inspector:
          group: image
          position: 100
    leftCaption:
      type: string
      ui:
        label: 'Vendor.Package:NodeTypes.ImageMixin:properties.caption'
        inspector:
          group: image
          position: 200
  
# Autogenerated Mixin of the second SuperType-Modification..
'Vendor.Package:ImageMixin-f6dc591593ef2c992ef29315ad58bf07a2b565a5':
  abstract: true
  properties:
    rightImage:
      type: Neos\Media\Domain\Model\ImageInterface
      ui:
        label: 'Vendor.Package:NodeTypes.ImageMixin:properties.image'
        inspector:
          group: image
          position: 100
    rightCaption:
      type: string
      ui:
        label: 'Vendor.Package:NodeTypes.ImageMixin:properties.caption'
        inspector:
          group: image
          position: 200
```

## Usage

```yaml
# Original..
 
'Vendor.Package:NodeType':
  superTypes:
    'Vendor.Package:MixinA': true
    'Vendor.Package:MixinB': false
  
  
# Disable just a small part..
  
'Vendor.Package:NodeType':
  superTypes:
    'Vendor.Package:MixinA': {ui: false}
    'Vendor.Package:MixinB':
      ui: false
  
  
# Give the generated SuperTypes a name..
  
'Vendor.Package:NodeType':
  superTypes:
    'Vendor.Package:Mixin':
      _name: 'Vendor.Package:Mixin-WithoutUI'
      ui: false
  
  
# Rename and/or disable some properties..
  
'Vendor.Package:NodeType':
  superTypes:
    'Vendor.Package:Mixin':
      properties:
        'propertyA': 'newPropertyA'
        'propertyB': false
  
  
# Rename all properties with a pre and/or suffix..
  
'Vendor.Package:NodeType':
  superTypes:
    'Vendor.Package:Mixin':
      properties:
        '*': 'prefix*Suffix'
  
  
# Use one mixin multiple times with renamed properties..
  
'Vendor.Package:NodeType':
  superTypes:
    'Vendor.Package:Mixin':
      -
        properties:
          'property': 'newPropertyA'
      -
        properties:
          'property': 'newPropertyB'
  
  
# Use one mixin multiple times with renamed properties and keep the original one..
  
'Vendor.Package:NodeType':
  superTypes:
    'Vendor.Package:Mixin':
      -
        properties:
          'property': 'newPropertyA'
      -
        '*': true
```

## Principles

- This package allows to generate dynamic Mixins out of existing ones, but with renamed or disabled parts in it
- It only changes values, if it's needed because of a renaming, deletion, repetition or to keep the original i18n translations
- Everything else has to be done at the right place for that.. in the NodeType-Definition itself

## ToDo

- Automatic group-renaming in the ui settings and for each corresponding property
- Automatic re-positioning of properties if one mixin is used multiple times
- Tests


------------------------------------------

developed by [webexcess GmbH](https://webexcess.ch/)
