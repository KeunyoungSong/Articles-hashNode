---
title: "[Material Design3]Icons"
datePublished: Tue Jan 09 2024 01:50:07 GMT+0000 (Coordinated Universal Time)
cuid: clr5p2inj000009l4gr6w4tlg
slug: material-design3icons

---

# Icon & Material Symbol styles

Material Symbols are the new default, and are available in three styles:

* outlined
    
* rounded
    
* sharp
    

> The legacy Material Icons continue to be available, but don’t have the variable font capabilities of Material Symbols.

## Outlined style

Outlined symbols use stroke and fill attributes for a light, clean style that works well in dense UIs.

![Outlined icons](https://lh3.googleusercontent.com/-bzCWxbAR_v0olEQs7E5PhPjn3PtHF56e_UwT_Fx8b7_wOjmrv3lHQaZNMoFpm1xGqXqEXL-_n0k3rZ1CXEWTa81JCCUxn6qrebPvrkGC70=s0 align="center")

> 2dp outlined icons readable across sizes and applications

![](https://lh3.googleusercontent.com/RmT1Ve3mQFKGAKDnrBxR9i_kI0ylBLnRuaZ1Xto5no-YFuwVHEwMqBto-X2J5lerIs-AfnmET5LY4cyPHNoc2J5KReVqagONYuPScNHSECy7=s0 align="left")

For optimal legibility and recognition, some symbols should remain filled, such as full body human icons or proprietary icons

![](https://lh3.googleusercontent.com/VdC6n5CA8BgBtdFCco7XMfNNjzF_aAr_HkS81AZMAezk6-QR4jUktg2WZs1Yt0zc2nejTJTLLKmaGr0UcQJVmUdEmeQbCxrE4dpcH-oNwMSTsA=s0 align="left")

![No.2](https://lh3.googleusercontent.com/Czg8IV24VI9ktFF08adu7HGrJCJ01kjZ729pkcX9D9oi94D07DdwTKEYRMZXdm4k721F6OaDDVRpBSbFlOvNUIiC5a-pMNcIO1dxfpjUnuuDPQ=s0 align="center")

The lighter stroke weight of these outlined symbols mirrors the thin lines of the app’s typography

## Rounded and sharp styles

Rounded symbols use a corner radius that pairs well with brands that use heavier typography, curved logos, or circular elements to express their style.

Sharp symbols display corners with straight edges, for a crisp style that remains legible even at smaller scales. These rectangular shapes can support brand styles that aren’t well-reflected by rounded shapes.

Rounded-style icons

![](https://lh3.googleusercontent.com/fgU9TUN5V2PJicYYUGm_WxSVsfns6J31ogK6WSI6ykhfj1A2M5WaubQippjNA5EBdSSJOOFIkSh2_o_giVn9E-dNVTCmufGahA1XiEutQBI=s0 align="left")

Sharp-style icons

![](https://lh3.googleusercontent.com/L-reOfntB8R9ysPCWMQtOJYdzSv18bWCUMIL5eHXcxND-L-ElgeNiovFCgxZ4GcWIfqt588epwcAURolqYDOea0T4sG-MWTVJGTjjrF-hZKuhQ=s0 align="left")

This app uses rounded buttons and round icons

![](https://lh3.googleusercontent.com/SujCWMK_APywTIKdwOKbK-DZzQzAF96VfBFUE2gzswTn8z_iNBQzcilQXjgVLp5xaTHzdo3OoNvblXWKK8GqY8u3n16e-Vec1V0GGckBP0G7qg=s0 align="left")

The 0dp corner radius of the sharp icon set echoes this app's rectangular design details

# Customizing Symbols

Material Symbols have four adjustable stylistic variable font attributes called **axes**.

> An axis is a typographic term referring to the attribute of a symbol that can be altered to create visual variations.

Each style symbol contains four axes:

* weight
    
* fill
    
* grade
    
* optical size
    

## Weight

Weight defines the symbol’s stroke weight, with a range of weights between thin (100) and bold (700). Weight can also affect the overall size of the symbol.

![](https://lh3.googleusercontent.com/vLGUsw44oYqs5OI1tqzTY660Ie4vyZD-bFJkM5_juY77Y5krvGCx6FxA0MlqPRk7-ah9I2JQDKuFJazFTeaqeG4ZbW-P5UWLEDXeWcjNn6zNkg=s0 align="left")

#### Don't

* Don't use the lightest weight(100) for standard-size (24dp) icons. The minimum weight for the size is 200.
    
* Don't mix different weight
    

#### Caution

* Be careful using excessive weight for standard 24dp symbols
    

#### Do

* Apply weights consistently
    

## fill

Fill gives you the ability to transition from a more outlined style to a reversed or more filled style.

**A fill attribute can be used to convey a state of transition**, such as unfilled and filled states. Values range from 0 to 1, with 1 being completely filled. Along with weight, fill is a primary attribute that impacts the overall look of a symbol.

Unfilled symbols with fill set to 0

![](https://lh3.googleusercontent.com/LBUWGaHfPou6Q5oGM7dzdF_WsJG0IaSaOXN_Q5YwSrOcD7cmgAqpDpGYv3tiaTbV5U_0yXbrq02-fHM1IEQBiJs0xDNL7MrJzhXdVHS7fdppGA=s0 align="left")

Filled symbols with fill set to 1

![](https://lh3.googleusercontent.com/RwlCtGh1pqG30Lw7WGEJavn5BNXWEJgMrtyWoeNYHdBM_L8dUm8fBWtkzF8FKxzfqsbWw8b__xrlSKX7qfC6OyL6ofstcn-KP6S9GcBUsB18Iw=s0 align="left")

## Grade

Weight and grade affect a symbol’s thickness. Adjustments to grade are more granular than adjustments to weight and have a smaller impact on the size of the symbol.

Grade is also available in some text fonts. Grade levels between text and symbols can be matched for a harmonious visual effect. For example, if the text font has a -25 grade value, the symbols can match it with a suitable value of -25.

![](https://lh3.googleusercontent.com/B2JQL_s6l3XjQDjGAqX8Q-W05NZ60G4j2yS_Hx50pU1Qd6HNnxPL4GGXKVhoVZVWKKfI4wkPB2F9nvhZkmJ0BkPnwaJBWQu8AEBynuYzdiSu=s0 align="center")

1. At grade 0, the thickness of the symbol does not change
    
2. At negative grade, the thickness of the symbol appears lighter
    

Grade can also compensate for **visual bleed**, which is when images can look bigger or smaller depending on the color contrast.

> To match the apparent icon size, the default grade for a dark icon on a light background is 0, and -25 for a light icon on a dark background.

Icon button featuring a 0 default grade symbol in light UI

![](https://lh3.googleusercontent.com/OlMXf-5Qc69FKAvL70A6dAQ8X_Vq3NiPK-snFI_CkHisWl1VV0huqHsQuIOk2CYlwag1TnZ7ZYLqmQ3d6mPJk4Mprsmnpt7kd_1ZHcxwgKg=s0 align="left")

Icon button featuring a negative grade symbol in dark UI

![](https://lh3.googleusercontent.com/HwOfcL4p1PEwsL-twSHasBauAcUdw56iiaPmq_RzL79LWyY5vSj4-tHOtPna-rfVYha5d2VJy7lLT5IxwgAz66jiV5MJjJ6Q9ufriXT08LgA2A=s0 align="left")

> To make strokes heavier and more emphasized, use positive value grade, such as when representing an active icon state.

An icon with active state using positive value grade for emplhasis

![](https://lh3.googleusercontent.com/HlIgsLqTk6V-mc7IVBtlPaqii3EMCfcEmQmqxXvEvhWiCZslOQQ5JtGaJ95bMEP7DTDP9UkWGJHgo6_z2fCYXbhQrv5jtKOb_uuhNVYuQERi=s0 align="left")

## Optical sizes

> Optical sizes range from 20dp to 48dp.
> 
> For the image to look the same at different sizes, the stroke weight (thickness) changes as the icon size scales. Optical size offers a way to automatically adjust the stroke weight when you increase or decrease the symbol sizes

Four optical sizes, 20dp, 24dp, 40dp, 48dp

![](https://lh3.googleusercontent.com/HOPC05vQ5XWnpO6eguPBVrvkRgtNhnIv7hllCJbFjnd0vWEQhVkogDeiHx47fh5Evb7XVKOaEtM18afa-JEFUoBDtBI_fIJSkjHRrkWKKoXwMA=s0 align="left")

Traditionally, icons are resized from a 24dp source vector, resulting in a large scaled icon that’s too heavy compared to the original. With the optical size axis, you can maintain the stroke weight (thickness) as the icon size grows.

![](https://firebasestorage.googleapis.com/v0/b/design-spec/o/projects%2Fm3%2Fimages%2Fl1f77lcp-opsz_comparison.gif?alt=media&token=0af2e5c1-aa7a-412b-a6f5-26bfc1f7cd1e align="left")

1. Material icon
    
2. Material Symbol
    

#### Use

* Use 20dp optical size value for dense layouts on desktop
    
* Use larger size 40dp~48dp symbols when primary actions need to be highlighted
    

# Using symbols with typography

Pair your type with Material Symbols. Symbols are designed with similar considerations to typefaces, and often appear alongside text. Choosing the right icon set can tie the content of an interface together, enhancing the cohesive branded feel of your product.

#### Don't

* Don't use the different weights for Material Symbols and text
    
* Don't use the same baseline for Material Symbols and text
    

#### Do

* Use the same weight for Material Symbols and Google Sans family text
    
* Shift down the baseline of symbols to approximately 11.5% of the type size
    

# Accessibility

## icons with a label text

Label text provides short, meaningful descriptions when symbols are more abstract. This can prove helpful in the case of navigation.

#### Do

* Label text provides short descriptions, especially useful for navigation
    

#### Caution

* Use caution if icons are displayed without labels. Icon meaning should always be unambiguous and accessible for all users. Text labels can be omitted in specific circumstances where reduced visual impact is necessary.
    

## Target size

Adequate space should surround icons to allow legibility and interaction.

> **Symbols of 24dp can use a target size of 48dp.**

![](https://lh3.googleusercontent.com/cpEWIYawy8hkPchF-pSFbqMai-GvH571Q_251r3auUkmDa_dpkGtInK_ivAh6mhWOrgo8K0x42wwx30l7JcyAGRzNAvvguhOaOot40sSZzyk=s0 align="left")

1. Clearance area
    
2. Placement
    

> When a *mouse and keyboard* are the primary input methods, measurements may be condensed to accommodate denser layouts.
> 
> A 20dp size symbol can use a target size of 40dp.

![](https://lh3.googleusercontent.com/jJIWGz6DwionLKH_r2AIRQ12oqRhQhR2QSSJj2g639iwZ79a_fd509CkpOufpRwJwFqXcZ_WiscgFKCQQxRNE5lsZv7OfehH6Pbk1o5maK_i=s0 align="left")

1. Clearance area
    
2. Placement