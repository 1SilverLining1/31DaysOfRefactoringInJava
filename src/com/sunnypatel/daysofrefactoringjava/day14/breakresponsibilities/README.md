When breaking apart responsibilities on a class this is enforcing Single Responsiblity Principle from SOLID.
It’s an easy approach to apply this refactoring although it’s often disputed as what consitutes a
“responsibility”. While I won’t be answering that here, I will show a clear cut example of a class that can be broken into several classes with specific responsibilities.

```Java
public class Customer {
	private List<Double> lateFees = new ArrayList<>();
	private List<Video> videos = new ArrayList<>();
	public List<Double> getLateFees() {
		return lateFees;
	}
	public void setLateFees(List<Double> lateFees) {
		this.lateFees = lateFees;
	}
	public List<Video> getVideos() {
		return videos;
	}
	public void setVideos(List<Video> videos) {
		this.videos = videos;
	}
}

public class Video {
	public void payFee(double fee){
		
	}
	public void rentVideo(Video video, Customer customer){
		List<Video> videos = customer.getVideos();
		videos.add(video);
		customer.setVideos(videos);
	}
	public double calculateBalance(Customer customer){
		double sum = 0 ;
		List<Double> lateFees = customer.getLateFees();
		for(double lateFee : lateFees){
			sum += lateFee;
		}
		return sum;
	}
}
```

As you can see here, the Video class has two responsibilities, once for handling video rentals, and another
for managing how many rentals a customer has. We can break out the customer logic into it’s own class to
help seperate the responsibilities.

```Java
public class Customer {
	private List<Double> lateFees = new ArrayList<>();
	private List<Video> videos = new ArrayList<>();
	
	public void payFee(double fee){
		
	}
	/*
	 * Not by Sunny:
	 * I don't like anything about calculateBalance method below!
	 * 1. Why does it reside in Customer, shouldn't there be a separate class - like a controller/manager?
	 * 2. Let's say it can only live here, then why does it need to take customer as a param? Can it calculate for "this"?
	 */
	public double caclulateBalance(Customer customer){
		double sum = 0 ;
		List<Double> lateFees = customer.getLateFees();
		for(double lateFee : lateFees){
			sum += lateFee;
		}
		return sum;
	}
	public List<Double> getLateFees() {
		return lateFees;
	}
	public void setLateFees(List<Double> lateFees) {
		this.lateFees = lateFees;
	}
	public List<Video> getVideos() {
		return videos;
	}
	public void setVideos(List<Video> videos) {
		this.videos = videos;
	}
}

public class Video {
	public void rentVideo(Video video, Customer customer){
		List<Video> videos = customer.getVideos();
		videos.add(video);
		customer.setVideos(videos);
	}
}

```
 
 #### original C# code from the book:
```cs
public class Video {
	public void PayFee(decimal fee) {
	}
	public void RentVideo(Video video, Customer customer) {
		customer.Videos.Add(video);
		}
	public decimal CalculateBalance(Customer customer) {
		return customer.LateFees.Sum();
	}
}
public class Customer {
	public IList<decimal> LateFees { get; set; }
	public IList<Video> Videos { get; set; }
}
public class Video {
	public void RentVideo(Video video, Customer customer) {
		customer.Videos.Add(video);
	}
}
public class Customer {
	public IList<decimal> LateFees { get; set; }
	public IList<Video> Videos { get; set; }
	public void PayFee(decimal fee) {
	}
	public decimal CalculateBalance(Customer customer) {
		return customer.LateFees.Sum();
	}
} 
```