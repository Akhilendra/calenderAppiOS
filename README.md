<h1>Calendar for iOS in Swift 4</h1>
 
This is the sourcecode for the app in my tutorial on Youtube https://youtu.be/srJj8U5d5ok
<br>
It can be used as a date picker in ios apps. You are free to use it in your projects.

<p align="center">
<img height="400" src="https://github.com/Akhilendra/calenderAppiOS/blob/master/Simulator%20Screen%20Shot%20-%20iPhone%206%20-%202017-10-22%20at%2010.13.26.png" />
&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp
<img height="400" src="https://github.com/Akhilendra/calenderAppiOS/blob/master/Simulator%20Screen%20Shot%20-%20iPhone%206%20-%202017-10-22%20at%2010.13.23.png" />
</p>

If you want to start the weekday from Monday rather than Sunday as in this project, below are the changes you need to make:

Step 1. In WeekdaysView.swift file, replace daysArr with following:

    var daysArr = ["Mo", "Tu", "We", "Th", "Fr", "Sa", "Su"]

Step 2. In CalenderView.swift file:
Replace function getFirstWeekDay with following:
    
    func getFirstWeekDay() -> Int {
        let day = ("\(currentYear)-\(currentMonthIndex)-01".date?.firstDayOfTheMonth.weekday)!      //1-7 -> Sun-Sat
        return day == 1 ? 8 : day
    }
    
Step 3. In CalenderView.swift file:
Replace numberOfItemsInSection and cellForItemAt delegate functions of CollectionView with following:
         
     func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
         let numDays = numOfDaysInMonth[currentMonthIndex-1]
         let num = numDays + firstWeekDayOfMonth - 2
         return num
     }
    
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell=collectionView.dequeueReusableCell(withReuseIdentifier: "Cell", for: indexPath) as! dateCVCell
        cell.backgroundColor=UIColor.clear
        if indexPath.item <= firstWeekDayOfMonth - 3 {
            cell.isHidden=true
        } else {
            let calcDate = indexPath.row-firstWeekDayOfMonth+3
            cell.isHidden=false
            cell.lbl.text="\(calcDate)"
            if calcDate < todaysDate && currentYear == presentYear && currentMonthIndex == presentMonthIndex {
                cell.isUserInteractionEnabled=false
                cell.lbl.textColor = UIColor.lightGray
            } else {
                cell.isUserInteractionEnabled=true
                cell.lbl.textColor = Style.activeCellLblColor
            }
        }
        return cell
    }
    
That's it. You're done.
