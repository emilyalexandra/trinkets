# Trinkets
A data-driven accessory mod for Minecraft using Fabric.

![Image of the Trinkets UI](https://i.imgur.com/CgWhc5a.png)

## About
Trinkets adds a slot group and slot system to Minecraft. Slot groups are collections of slots for a certain body part. By default there are 6 slot groups (head, chest, legs, feet, offhand, hand) that can have slots added to them, but more groups can be added if desired. By itself, Trinkets only implements one slot (chest:cape) and reimplements the elytra to use this slot, mods are responsible for adding any slots they plan on using.

## Suggested Slots
If you're a developer using Trinkets for your mod, here is the list of slots I suggest you use before creating new ones. These slots are not enabled by default but have textures provided by Trinkets and should be used for the most compatibility between mods. This list will be added to and modified over time.

| Slot | Examples | Texture identifier
| --- | --- | --- |
| `head:mask` | Masks, glasses, monocles | `new Identifier("trinkets", "textures/item/empty_trinket_slot_mask.png")`
| `chest:backpack` | Backpack | `new Identifier("trinkets", "textures/item/empty_trinket_slot_backpack.png")`
| `chest:cape` | Capes, wings | `new Identifier("trinkets", "textures/item/empty_trinket_slot_cape.png")`
| `chest:necklace` | Necklaces, collars | `new Identifier("trinkets", "textures/item/empty_trinket_slot_necklace.png")`
| `legs:belt` | Belts | `new Identifier("trinkets", "textures/item/empty_trinket_slot_belt.png")`
| `feet:aglet` | Aglets | `new Identifier("trinkets", "textures/item/empty_trinket_slot_aglet.png")`
| `offhand:ring` | Rings, other jewelery | `new Identifier("trinkets", "textures/item/empty_trinket_slot_ring.png")`
| `hand:gloves` | Gloves | `new Identifier("trinkets", "textures/item/empty_trinket_slot_gloves.png")`
| `hand:ring` | Rings, other jewelery | `new Identifier("trinkets", "textures/item/empty_trinket_slot_ring.png")`

## Developers
To add Trinkets to your project you need to add jitpack and nerdhubmc to your repositories in your build.gradle
```
repositories {
	maven {
		name = "TerraformersMC"
		url = "https://maven.terraformersmc.com/"
	}
	maven {
		name = "NerdHubMC"
		url = "https://maven.abusedmaster.xyz/"
	}
}
```
And then to add Trinkets you add it as a dependency in your build.gradle
```
dependencies {
	modImplementation "dev.emi:trinkets:${trinkets_version}"
}
```
All you need to get started with Trinkets is to register a slot and add an item to the game that extends `TrinketItem`, here is a short example:
```java
//Inside your ModMain
public void onInitialize() {
	//Register your item somewhere
	//Adding the hand:ring slot to Trinkets, as it does not exist by default, note that this uses the provided Trinkets texture
	TrinketSlots.addSlot(SlotGroups.HAND, Slots.RING, new Identifier("trinkets", "textures/item/empty_trinket_slot_ring.png"));
}
```
```java
//A class for your trinket
public class RingItem extends TrinketItem {

	public RingItem() {
		//Adding the trinket to the TOOLS group and making it not stack
		super(new Settings().group(ItemGroup.TOOLS).maxCount(1));
	}

	@Override
	public boolean canWearInSlot(String group, String slot) {
		//Determines what slots the trinket can be worn in, this makes it usable in the hand:ring slot
		return group.equals(SlotGroups.HAND) && slot.equals(Slots.RING);
	}

	@Override
	public void tick(PlayerEntity player, ItemStack stack) {
		//Just one of the methods you can override in Trinket, the ring gives you the speed effect while wearing it
		//Though you probably shouldn't give the player a status effect every tick
		player.addStatusEffect(new StatusEffectInstance(StatusEffects.SPEED, 19, 0));
	}
}
```
Something important to note is that trinkets that extend `TrinketItem` will automatically be equippable by dispensers and right click. If you make an item that instead implements the base interface `Trinket`, you will need to add this functionality yourself.

## Other Information
For a tutorial on rendering trinkets, visit this repository's [wiki](https://github.com/emilyalexandra/trinkets/wiki/Rendering-Trinkets).
