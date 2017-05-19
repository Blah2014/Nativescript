# Nativescript

### iOS: overwrite the current delegate for the SearchBar component

```xml
<Page xmlns="http://schemas.nativescript.org/tns.xsd" navigatingTo="navigatingTo" class="page">

    <Page.actionBar>
        <ActionBar title="My App" icon="" class="action-bar">
        </ActionBar>
    </Page.actionBar>

    <GridLayout rows="auto *" columns="*" class="p-20">
        <SearchBar loaded="searchBarLoaded" row="0" col="0" id="searchBar" hint="Search" text="" clear="onClear" submit="onSubmit" />
    </GridLayout>
</Page>
```
