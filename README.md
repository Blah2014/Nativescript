# Nativescript

### iOS: overwrite the current delegate for the SearchBar component

XML
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

TypeScript
```javascrit
import { EventData } from 'data/observable';
import { Page } from 'ui/page';
import { HelloWorldModel } from './main-view-model';
import {SearchBar} from "ui/search-bar";
import {TextField} from "ui/text-field"
import { isIOS } from "platform";


export function navigatingTo(args: EventData) {

    let page = <Page>args.object;

    page.bindingContext = new HelloWorldModel();
}

export function onClear(args){
    var search:SearchBar = <SearchBar>args.object;
    setTimeout(()=>{
        search.dismissSoftInput();
    }, 0)
}


export function tap(args){
var textObj:TextField = <TextField> args.object;
console.log("tap");
console.log(textObj);
}

export function searchBarLoaded(args){
    var searchbar:SearchBar = <SearchBar>args.object;
    console.log("searchbar loaded");

        console.log(searchbar.ios);
        searchbar.ios.setShowsCancelButtonAnimated(true, true)
        let newDelegate = newUISearchBarDelegate.initWithOriginalDelegate((<any>searchbar)._delegate);
        (<any>searchbar)._delegate=newDelegate;
    

}
class newUISearchBarDelegate extends NSObject implements UISearchBarDelegate {

    public static ObjCProtocols = [UISearchBarDelegate];

    private _originalDelegate: UISearchBarDelegate;

    public static initWithOriginalDelegate(originalDelegate: UISearchBarDelegate): newUISearchBarDelegate {
        console.log("initWithOwner")

        let delegate = <newUISearchBarDelegate>newUISearchBarDelegate.new();
        delegate._originalDelegate = originalDelegate;
    
        console.log(delegate);
        console.log(delegate._originalDelegate);
        return delegate;
    }


    public searchBarTextDidChange(searchBar: UISearchBar, searchText: string) {
        return this._originalDelegate.searchBarTextDidChange(searchBar, searchText);

    }

    public searchBarCancelButtonClicked(searchBar: UISearchBar) {
        console.log("searchBarCancelButtonClicked");
        this._originalDelegate.searchBarCancelButtonClicked(searchBar);
    }

    public searchBarSearchButtonClicked(searchBar: UISearchBar) {
        this._originalDelegate.searchBarSearchButtonClicked(searchBar);
    }
}
```

### iOS (NativeScript Angular 2): overwrite the current delegate for the SearchBar component

HTML
```html
<ActionBar title="My App" class="action-bar">
</ActionBar>
<StackLayout class="page">
   
   <GridLayout rows="auto *" columns="*" class="p-20">
       <SearchBar (loaded)="searchBarLoaded($event)" row="0" col="0" id="searchBar" hint="Search" text="" class="search-bar"></SearchBar>
    </GridLayout>
</StackLayout>
```

TypeScript
```javascript
import { Component, OnInit } from "@angular/core";
import {SearchBar} from "ui/search-bar";


@Component({
    selector: "ns-items",
    moduleId: module.id,
    templateUrl: "./items.component.html",
})
export class ItemsComponent implements OnInit {

    constructor() { }

    ngOnInit(): void {
    }
    searchBarLoaded(args){
        var searchbar:SearchBar = <SearchBar>args.object;
        console.log("searchbar loaded");

            console.log(searchbar.ios);
            searchbar.ios.setShowsCancelButtonAnimated(true, true)
            let newDelegate = newUISearchBarDelegate.initWithOriginalDelegate((<any>searchbar)._delegate);
            (<any>searchbar)._delegate=newDelegate;
    }
}

class newUISearchBarDelegate extends NSObject implements UISearchBarDelegate {

    public static ObjCProtocols = [UISearchBarDelegate];

    private _originalDelegate: UISearchBarDelegate;

    public static initWithOriginalDelegate(originalDelegate: UISearchBarDelegate): newUISearchBarDelegate {
        console.log("initWithOwner")

        let delegate = <newUISearchBarDelegate>newUISearchBarDelegate.new();
        delegate._originalDelegate = originalDelegate;
    
        console.log(delegate);
        console.log(delegate._originalDelegate);
        return delegate;
    }


    public searchBarTextDidChange(searchBar: UISearchBar, searchText: string) {
        return this._originalDelegate.searchBarTextDidChange(searchBar, searchText);

    }

    public searchBarCancelButtonClicked(searchBar: UISearchBar) {
        console.log("searchBarCancelButtonClicked");
        this._originalDelegate.searchBarCancelButtonClicked(searchBar);
    }

    public searchBarSearchButtonClicked(searchBar: UISearchBar) {
        this._originalDelegate.searchBarSearchButtonClicked(searchBar);
    }
}
```

Bear in mind that you should also install tns-platform-declarations plugin. More about setting up those declarations could be found [here](https://www.npmjs.com/package/tns-platform-declarations).
