This library is meant to be included with projects that would like to set extra item filtering rules for various scenes in the game. You should also include the version of LibStub packaged with libFilters.

###Including the library
To include the library in your project, copy the provided LibStub and libFilters folders into your project's directory. Then add the following lines to your project's manifest file:

    ## OptionalDependsOn: libFilters
    
    path\from\project\root\to\LibStub\LibStub.lua
    path\from\project\root\to\libFilters\libFilters.lua

###Initialization
To initialize libFilters, simply make a call to LibStub:

    local libFilters = LibStub("libFilters")
    
###Register and Unregister additional filters
Filter (un)registration works just like (un)registering for ESO events.

To register a new filter, call:

    libFilters:RegisterFilter(filterTag, filterType, filterCallback)
    
filterTag should be a unique string.

filterCallback is the name of a function that contains the new filtering logic you would like to apply. A simple function could look like:

    local function exampleFunction(itemData)
        if itemData.itemType == ITEMTYPE_ARMOR then
            return true --returning true means the item is displayed
        else
            return false --returning false means the item is not displayed
        end

filterType is any one of the LAF_ prefixed constants provided by libFilters. They are:

    LAF_BAGS = 1
    LAF_BANK = 2
    LAF_GUILDBANK = 3
    LAF_STORE = 4
    LAF_DECONSTRUCTION = 5
    LAF_GUILDSTORE = 6
    LAF_MAIL = 7
    LAF_TRADE = 8
    LAF_ENCHANTING_CREATION = 11
    LAF_ENCHANTING_EXTRACTION = 12
    LAF_IMPROVEMENT = 13
    LAF_FENCE = 14
    LAF_LAUNDER = 15

To unregister a filter, call:

    libFilters:UnregisterFilter(filterTag, filterType)

If you need to refresh the inventory you applied filters to in order for them to take effect, call:

    libFilters:RequestInventoryUpdate(filterType)
    
###Other libFilters functions

libFilters has other functions that may prove useful to your needs.

    libFilters:HookAdditionalFilter(filterType, inventory)
    --inventory should be one of:
    --a) backpack layout fragment with layoutData member
    --b) inventory table from PLAYER_INVENTORY.inventories
    
    libFilters:RequestInventoryUpdate(filterType)
    --manually request an update for the inventory tied to the provided LAF_ prefixed constant
    
    libFilters:IsFilterRegistered(filterTag, filterType)
    --returns true if the filtering logic registered with filterTag is still active, false otherwise
    
    libFilters:GetCurrentLAF(inventoryType)
    --returns the LAF_ prefixed constant that is associated with the provided inventoryType and the currently viewed scene
    
    libFilters:InventoryTypeToLAF(inventoryType)
    --returns the LAF_ prefixed constant that is associated with the provided inventoryType
    --returns 0 if inventoryType is incorrect
    
    libFilters:BagIdToLAF(bagId)
    --returns the LAF_ prefixed constant that is associated with the provided bagId
    --returns 0 if bagId is incorrect
