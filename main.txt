/****************
=================== CIS 400/700 ASSIGNMENT COVER SHEET =================== 

1.  Assignment (e.g, Homework 17): Homework 3

2a. Name & email address (First author): Nicholas J. Saeli

2b. Name & email address (Second author, if any):

3a. Did you consult with anyone on parts of this assignment?
    (Yes/No): No

3b. If so, give the details of these consultations (e.g., who, what,
    where)? N/A


4a. Did you consulted an outside source on parts of this assignment 
    (e.g., an Internet site)?  (Yes/No): Yes

4b. If so, give the details of these consultations (e.g., who, what, 
    where)? Rust Documentation

5.  The authors attest that the above is correct, (Yes/No): Yes 

=====================================================================
*****************/
use std::cmp;

fn main() {
    let initial = [66, 70, 52, 93, 44, 67, 47, 10, 11, 13, 94, 99, 51, 12];
    let mut to_sort;
    println!("initial:          {:?}",initial);

    to_sort  = initial.clone();
    bubble_sort(&mut to_sort);
    println!("bubble-sorted:    {:?}",to_sort);

    to_sort  = initial.clone();
    sel_sort(&mut to_sort);
    println!("selection-sorted: {:?}",to_sort);

    to_sort  = initial.clone();
    insert_sort(&mut to_sort);
    println!("insertion-sorted: {:?}",to_sort);

    println!();
    println!("unordered search:");
    report_search(44,unordered_search(44,&initial[..]));
    report_search(43,unordered_search(43,&initial[..]));

    println!();
    println!("binary search:");
    report_search(44,binary_search(44,&to_sort[..]));
    report_search(43,binary_search(43,&to_sort[..]));

    println!();
    println!("the min and max of initial are {:?}",
             min_max(&initial[..]));
}

/*
// NOTE!! The following will not compile: It needs lifetime annotations.
// We'll fix this later on.
fn swap(x :&mut u32, y : &mut u32) {
    let t = x;
    x = y;
    y = t;
}
*/

fn bubble_sort(a : &mut [u32]) {
    let len = a.len();
    for i in 0..len {
        for j in 0..(len-i-1) {
            if a[j]>a[j+1] {
                // swap the values of a[j] and a[j+1]
                let t = a[j];
                a[j] = a[j+1];
                a[j+1] = t;
            }
        }
    }
}

fn report_search(x : u32, r : Option<usize>) {
    print!("\t {} ",x);
    match r {
        None    => { println!("not found"); },
        Some(i) => { println!("found at index {}",i); },
    }
}

fn unordered_search(x : u32, a : &[u32]) -> Option<usize> {
    for i in 0..a.len() {
        if x==a[i] { return Some(i); }
    }
    None
}


fn sel_sort(a : &mut [u32]) {
    let n = a.len();
    let mut i = 0;
    let mut j = 0;

    for j in j..(n-1){
        let mut iMin = j;
        i = j+1;
        for i in i..n {
            if(a[i]<a[iMin]){
                iMin = i;
            }
        }

        if(iMin != j){
            let swap = a[j];
            a[j] = a[iMin];
            a[iMin] = swap;
        }
    }
}

fn insert_sort(a : &mut [u32]) {
    let n = a.len();
    let mut i = 0;
    for i in i..n {
        let mut j = i;
        while((j>0) && (a[j-1]>a[j])){
            let swap = a[j];
            a[j] = a[j-1];
            a[j-1] = swap;
            j-=1;
        }
    }
}

fn binary_search(x : u32, a : &[u32]) -> Option<usize> {
    let mut l = 0;
    let mut r = a.len()-1;
    while(l<=r){
        let m = ((l+r)/2);
        if(a[m]<x){
            l = m+1;
        }
        else if(a[m]>x){
            r = m-1;
        }
        else{
            return Some(m);
        }
    }
    return None;
}

fn min_max(a : &[u32]) -> (u32,u32) {
    let len = a.len();
    let x = 0;
    let y = len-1;
    assert!(len>0);
    if(y-x <= 1){
        return(Ord::min(a[x],a[y]),Ord::max(a[x],a[y]));
    }
    let (min1,max1) = maxmin(a,x,(x+y)/2);
    let (min2,max2) = maxmin(a,((x+y)/2)+1,y);
    return (Ord::min(min1,min2),Ord::max(max1,max2));
}

fn maxmin(a : &[u32], x : usize, y : usize) -> (u32,u32){
    if(y-x <= 1){
        return(Ord::min(a[x],a[y]),Ord::max(a[x],a[y]));
    }
    let (min1,max1) = maxmin(a,x,(x+y)/2);
    let (min2,max2) = maxmin(a,((x+y)/2)+1,y);
    return (Ord::min(min1,min2),Ord::max(max1,max2));
}
    
// NOTE:
// cmp::min(a,b) returns the minimum of a and b
// cmp::max(a,b) returns the maximum of a and b

/****************
=========================== TEST RUNS =============================
initial:          [66, 70, 52, 93, 44, 67, 47, 10, 11, 13, 94, 99, 51, 12]
bubble-sorted:    [10, 11, 12, 13, 44, 47, 51, 52, 66, 67, 70, 93, 94, 99]
selection-sorted: [10, 11, 12, 13, 44, 47, 51, 52, 66, 67, 70, 93, 94, 99]
insertion-sorted: [10, 11, 12, 13, 44, 47, 51, 52, 66, 67, 70, 93, 94, 99]

unordered search:
	 44 found at index 4
	 43 not found

binary search:
	 44 found at index 4
	 43 not found

the min and max of initial are (10, 99)
=====================================================================
*****************/