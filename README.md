# DSA
#Assignment

#include<iostream>
#include<chrono> // for time
#include<time.h> // for changin seed
#include<cstdlib> // for rng

void mergesort(int* arr,int size);
void merge(int* arr,int* left,int* right, int leftsize,int rightsize);
int binarySearch(int* arr,int search,int size);
void preety_print(int* arr,int size);
void selectionSort(int* arr,int size);
int interpolationSearch(int* arr,int size,int search);
void reset(int* copy, int* array,int size){
    for(int i=0;i<size;i++){array[i]=copy[i];}
}

using namespace std;
int main(){
    int n,sortedFlag=-1,search,searchOut;
    chrono::time_point<std::chrono::_V2::system_clock, std::chrono::_V2::system_clock::duration> start,end;
    chrono::duration<double> duration;
    cout<<"\nEnter the size of array: ";
    cin >>n;
    int array[n];
    srand(time(0));
    int negRandomness;
    for(int i=0;i<n;i++){
        negRandomness=(int) rand();
        array[i]=(int)rand();
        if(negRandomness%2!=0){
            array[i]-=(array[i]*2);
        }
        
    }

    int copy[n];
    for(int i=0;i<n;i++){
        copy[i]=array[i];
    }
    
    

    int menuChoice,sortmenuChoice,searchmenuChoice;
    Menu:
    cout<<"\n*******MENU*******"<<endl;
    cout<<"1.Sorting."<<endl;
    cout<<"2.Searching on Sorted Array."<<endl;
    cout<<"3.Print the Array."<<endl;
    cout<<"4.Reset the array."<<endl;
    cout<<"5.Clear Screen"<<endl;
    cout<<"6.Exit."<<endl;
    cout<<"Enter your choice (1-6): ";
    cin>>menuChoice;
    switch (menuChoice)
    {
    case 1 :
        goto SortMenu;

    case 2 :
        goto SearchMenu;
        break;
    
    case 3 :
        preety_print(array,n);
        break;

    case 4 :
        reset(copy,array,n);
        break;

    case 5:
        #if __WIN32
            system("cls");
        #else
            system("clear");
        #endif
        break;
    case 6 :
        cout<<"\bBYE!";
        return 0;
    default:
        cout<<"\nInput Out of bounds!!"<<endl;
        break;
    }
    goto Menu;


    SortMenu:
    cout<<"\n*****Sorting Options******"<<endl;
    cout<<"1.Selection Sort"<<endl;
    cout<<"2.Merge Sort"<<endl;
    cout<<"3.Back"<<endl;
    cout<<"4.Exit"<<endl;
    cout<<"Enter your Choise (1-4): ";
    cin>>sortmenuChoice;
    switch (sortmenuChoice)
    {
    case 1:
        if(sortedFlag==0){
            cout<<"\nArray was already sorted!, Resetting the Array!"<<endl;
            reset(copy,array,n);
            sortedFlag=-1;
        }
        start=chrono::high_resolution_clock::now();
        selectionSort(array,n);
        end=chrono::high_resolution_clock::now();
        duration=end-start;
        cout<<"\nTime taken by Selection Sort: "<<duration.count()<<endl;
        sortedFlag=0;
        break;
    
    case 2:
        if(sortedFlag==0){
            cout<<"\nArray was already sorted!, Resetting the Array!"<<endl;
            reset(copy,array,n);
            sortedFlag=-1;
        }
        start=chrono::high_resolution_clock::now();
        mergesort(array,n);
        end=chrono::high_resolution_clock::now();
        duration=end-start;
        cout<<"\nTime taken by Merge Sort: "<<duration.count()<<endl;
        sortedFlag=0;
        break;

    case 3:
        goto Menu;

    case 4:
        return 0;
    default:
        cout<<"\nInvalid Choice!!. Retry"<<endl;
        break;
    }
    goto SortMenu;


    SearchMenu:
    cout<<"\n*****Searching Options******"<<endl;
    cout<<"1.Binary Search"<<endl;
    cout<<"2.Interpolation Search"<<endl;
    cout<<"3.Back"<<endl;
    cout<<"4.Exit"<<endl;
    cout<<"Enter your Choise (1-4): ";
    cin>>searchmenuChoice;
    switch (searchmenuChoice)
    {
    case 1:
        if(sortedFlag!=0){
            cout<<"\nArray isn't sorted!, sorting first!"<<endl;
            mergesort(array,n);
            sortedFlag=0;
        }
        cout<<"\nEnter a value to search: "<<endl;
        cin>>search;
        start=chrono::high_resolution_clock::now();
        searchOut=binarySearch(array,search,n);
        end=chrono::high_resolution_clock::now();
        duration=end-start;
        if(searchOut<=0){
            cout<<"\nValue Not found!"<<endl;
        }
        else{
            cout<<"\n"<<search<<" is at index "<<searchOut<<endl;
        }
        cout<<"\nTime taken by Binary Search: "<<duration.count()<<endl;
        break;
    
    case 2:
        if(sortedFlag!=0){
            cout<<"\nArray isn't sorted!, sorting first!"<<endl;
            mergesort(array,n);
            sortedFlag=0;
        }
        cout<<"\nEnter a value to search: "<<endl;
        cin>>search;
        start=chrono::high_resolution_clock::now();
        searchOut=interpolationSearch(array,n,search);
        end=chrono::high_resolution_clock::now();
        duration=end-start;
        if(searchOut<=0){
            cout<<"\nValue Not found!"<<endl;
        }
        else{
            cout<<"\n"<<search<<" is at index "<<searchOut<<endl;
        }
        cout<<"\nTime taken by Interpolation Search: "<<duration.count()<<endl;
        break;

    case 3:
        goto Menu;

    case 4:
        return 0;
    default:
        cout<<"\nInvalid Choice!!. Retry"<<endl;
        break;
    }
    goto SearchMenu;

    return 0;
}

void preety_print(int* arr,int size){
    for(int i=0;i<size;i++){
        cout<<"\nElement no "<<i+1<<": "<<arr[i];
    }
    cout<<"\n";
}

void mergesort(int* arr,int size){
    if (size<2){return;}

    int mid=size/2;
    int* left=new int[mid];
    int* right=new int[size-mid];

    for(int i=0;i<mid;i++){
        left[i]=arr[i];
    }

    
    for(int i=mid;i<size;i++){
        right[i-mid]=arr[i];
    }
    mergesort(left,mid);
    mergesort(right,(size-mid));

    merge(arr,left,right,mid,size-mid);

    delete[] left;
    delete[] right;
}

void merge(int* arr,int* left,int* right, int leftsize,int rightsize){
    int i=0,j=0,k=0;

    while(i<leftsize && j<rightsize){
        if(left[i]<right[j]){
            arr[k++]=left[i++];
        }
        else{
            arr[k++]=right[j++];
        }
    }
    while(i<leftsize){
        arr[k++]=left[i++];
    }
    
    while(j<rightsize){
        arr[k++]=right[j++];
    }

}

void selectionSort(int* arr,int size){
    int swap;
    for(int left=0;left<size;left++){
        int temp=left;
        for(int i=left;i<size;i++){
            if(arr[i]<arr[temp]){
                temp=i;
            }
        }
        swap=arr[temp];
        arr[temp]=arr[left];
        arr[left]=swap;
    }
}

int binarySearch(int* arr,int search,int size){
    int left=0,right=size-1,mid;
    mid=(right/2)+left;
    while(left<=mid && right>=mid){
        if(search==arr[mid]){return mid;}
        else if(search<arr[mid]){
            right=mid-1;
        }
        else{
            left=mid+1;
        }
        mid=((right-left)/2)+left;
    }
    return -1;
}

int interpolationSearch(int* arr,int size,int search){
    int low=0,high=size-1;
    int probe=(high-low)/2+low;
    while(search>=arr[low]&&search<=arr[high]&&low<=high){
        probe=low+(((high-low)*(search-arr[low]))/(arr[high]-arr[low]));
        if(arr[probe]==search){return probe;}
        else if(search<arr[probe]){
            high=probe-1;
            continue;
        }
        low=probe+1;
    }
    return -1;
}
