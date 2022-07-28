## Simulate the elevator design
We will be simulating the scenario-

- A person is on a particular floor. Suppose Ground Floor. He wants to go to 5th Floor. So he clicks on the elevator button with up direction.
- We will be calling this the **ExternalRequest**. This Request will be having the direction and the floor on which the button has been pressed by the user i.e. source floor. The elevator will check the available requests if any and then process this request depending on some priority. The elevator reaches the source floor i.e. the 0th or the ground floor. The person enters the elevator.
- The person enters the elevator. The person then presses the 5th floor button in the elevator to indicate the elevator to go to 5th floor.
- This will be the **internal request**. So the internal request will be having only the floor to which the person wants to go to i.e. the destination floor. The elevator moves to the fifth floor. And the person then exits the elevator.
- If suppose when the elevator is moving (UP) from the ground floor to the fifth floor and it reaches the first floor. At this moment suppose another person on the second floor want’s to go in the UP direction. Then the elevator will stop for this request and the person on second floor will enter the elevator. Suppose he presses the 4th floor button. Then the elevator will first stop at 4th floor which was the destination of the person which entered on the second floor. Later the elevator stops on the fifth floor and the person from the ground floor exits.
- If suppose when the elevator is moving from the ground floor to the fifth floor and it reaches the first floor. At this moment suppose another person on the second floor want’s to go in the DOWN direction. Then the elevator will not stop for this request immediately. Elevator will first go to the fifth floor where the person from the ground floor will exit. Elevator will then go to the second floor. The person will enter the elevator and press 0. The elevator will then move to the zeroth floor.

```python
import enum
import heapq

class Direction(enum.Enum):
    UP = 1
    DOWN = 2
    IDLE = 0

class Location(enum.Enum):
    INSIDE_ELEVATOR = 1
    OUTSIDE_ELEVATOR = 2
    
class Request:
    def __init__(self, current_floor, dest_floor, direction, location):
        self.current_floor = current_floor
        self.dest_floor = dest_floor
        self.direction = direction
        self.location = location
    def __lt__(self, req):
        return self.dest_floor < req.dest_floor
        
class UpRequest(Request):
    def __init__(self, current_floor, dest_floor, direction, location):
        Request.__init__(self, current_floor, dest_floor, direction, location)
    
    def __lt__(self, req):
        return self.dest_floor < req.dest_floor

class DownRequest(Request):
    def __init__(self, current_floor, dest_floor, direction, location):
        Request.__init__(self, current_floor, dest_floor, direction, location)
    def __lt__(self, req):
        return self.dest_floor > req.dest_floor


class Elevator:
    def __init__(self, current_floor):
        self.current_floor = current_floor
        self.direction = Direction.IDLE
        self.up_queue = []
        self.down_queue = []
        
    def run(self):
        while len(self.up_queue) > 0 or len(self.down_queue) > 0:
            self.process_requests()
        
        print("Finished Processing all requests")
        self.direction = Direction.IDLE
    
    def process_requests(self):
        if self.direction == Direction.IDLE or self.direction == Direction.UP:
           self.process_up_request()
           self.process_down_request()
        else:
            self.process_down_request()
            self.process_up_request()
           
        
    def send_up_request(self, request):
        if request.location == Location.OUTSIDE_ELEVATOR:
            up_request = Request(request.current_floor, request.current_floor, Direction.UP, Location.OUTSIDE_ELEVATOR)
            self.up_queue.append(up_request)
            #heappq.heapify(self.up_request)
        
        self.up_queue.append(request)
        heapq.heapify(self.up_queue)
            
    
    def send_down_request(self, request):
        if request.location == Location.OUTSIDE_ELEVATOR:
            d_request = Request(request.current_floor, -request.current_floor, Direction.DOWN, Location.OUTSIDE_ELEVATOR)
            self.down_queue.append(d_request)
            #heappq.heapify(self.up_request)
        
        request.dest_floor = -request.dest_floor
        self.down_queue.append(request)
        heapq.heapify(self.down_queue)

    
    def process_up_request(self):
        print("Processing up request len is", len(self.up_queue))
        while len(self.up_queue) > 0:
            near_request = heapq.heappop(self.up_queue)
            self.current_floor = near_request.dest_floor
            print("Processing up requests. Elevator stopped at floor ",self.current_floor) 
        
        if len(self.down_queue) > 0:
            self.direction = Direction.DOWN
        else:
            self.direction = Direction.IDLE
    
    def process_down_request(self):
        print("Processing down request len is", len(self.down_queue))
        while len(self.down_queue) > 0:
            near_request = heapq.heappop(self.down_queue)
            self.current_floor = -near_request.dest_floor
            print("Processing down requests. Elevator stopped at floor ",self.current_floor) 
        
        if len(self.up_queue) > 0:
            self.direction = Direction.UP
        else:
            self.direction = Direction.IDLE
        


def driver():
    elevator = Elevator(0)

    upRequest1 = Request(elevator.current_floor, 5, Direction.UP, Location.INSIDE_ELEVATOR);
    upRequest2 = Request(elevator.current_floor, 3, Direction.UP, Location.INSIDE_ELEVATOR);

    downRequest1 = Request(elevator.current_floor, 1, Direction.DOWN, Location.INSIDE_ELEVATOR);
    downRequest2 = Request(elevator.current_floor, 2, Direction.DOWN, Location.INSIDE_ELEVATOR);
    downRequest3 = Request(4, 0, Direction.DOWN, Location.OUTSIDE_ELEVATOR);

    # Two people inside of the elevator pressed button to go up to floor 5 and 3.
    elevator.send_up_request(upRequest1);
    elevator.send_up_request(upRequest2);

    # One person outside of the elevator at floor 4 pressed button to go down to floor 0
    elevator.send_down_request(downRequest3);

    # Two person inside of the elevator pressed button to go down to floor 1 and 2.
    elevator.send_down_request(downRequest1);
    elevator.send_down_request(downRequest2);
    
    elevator.run()
    
driver()
```
