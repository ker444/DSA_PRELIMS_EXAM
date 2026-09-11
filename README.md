# PART 1: STUDENT RECORD MANAGER (DYNAMIC ARRAY ADT)

class StudentRecord:
    def __init__(self, id_num: str, full_name: str, degree_program: str, year: int):
        self.id_num = id_num
        self.full_name = full_name
        self.degree_program = degree_program
        self.year = year

    def info(self) -> str:
        return f"Student ID: {self.id_num} | Name: {self.full_name} | Course: {self.degree_program} | Year Level: {self.year}"


class CustomDynamicArray:
    def __init__(self):
        self.max_capacity = 5
        self.item_count = 0
        self.buffer = [None] * self.max_capacity

    def append_student(self, record: StudentRecord):
        if self.item_count == self.max_capacity:
            self.expand_capacity()
        self.buffer[self.item_count] = record
        self.item_count += 1

    def expand_capacity(self):
        new_max = self.max_capacity * 2
        new_buffer = [None] * new_max
        for idx in range(self.item_count):
            new_buffer[idx] = self.buffer[idx]
        self.buffer = new_buffer
        self.max_capacity = new_max
        print(f"\n[NOTICE] Array capacity expanded to {self.max_capacity}")

    def print_all(self):
        if self.item_count == 0:
            print("No records to display.")
            return
        print("\n=== REGISTERED STUDENTS ===")
        for idx in range(self.item_count):
            print(f"[{idx + 1}] {self.buffer[idx].info()}")

    def find_index(self, id_num: str) -> int:
        for idx in range(self.item_count):
            if self.buffer[idx].id_num.strip().lower() == id_num.strip().lower():
                return idx
        return -1

    def fetch(self, idx: int):
        if 0 <= idx < self.item_count:
            return self.buffer[idx]
        return None

    def edit_student(self, id_num: str, new_name: str, new_course: str, new_year: int) -> bool:
        target_idx = self.find_index(id_num)
        if target_idx != -1:
            self.buffer[target_idx].full_name = new_name
            self.buffer[target_idx].degree_program = new_course
            self.buffer[target_idx].year = new_year
            return True
        return False

    def delete_student(self, id_num: str) -> bool:
        target_idx = self.find_index(id_num)
        if target_idx == -1:
            return False

        for idx in range(target_idx, self.item_count - 1):
            self.buffer[idx] = self.buffer[idx + 1]

        self.buffer[self.item_count - 1] = None
        self.item_count -= 1
        return True

    def get_length(self) -> int:
        return self.item_count

    def get_capacity(self) -> int:
        return self.max_capacity


def get_valid_number(msg: str) -> int:
    while True:
        try:
            return int(input(msg).strip())
        except ValueError:
            print("Invalid input. Please enter a valid numerical value.")


def run_student_manager():
    array_adt = CustomDynamicArray()

    while True:
        print("\n---------------------------------")
        print("    STUDENT RECORD MANAGER       ")
        print("---------------------------------")
        print("1. Add Student")
        print("2. Display Students")
        print("3. Search Student")
        print("4. Update Student")
        print("5. Remove Student")
        print("6. Display Array Information")
        print("7. Exit")

        user_selection = get_valid_number("Enter your choice: ")

        if user_selection == 1:
            student_id = input("Enter Student ID: ").strip()
            if array_adt.find_index(student_id) != -1:
                print("Error: Student ID already registered.")
                continue

            student_name = input("Enter Student Name: ").strip()
            course_name = input("Enter Course: ").strip()
            year_level = get_valid_number("Enter Year Level: ")

            array_adt.append_student(StudentRecord(student_id, student_name, course_name, year_level))
            print("Student added successfully!")

        elif user_selection == 2:
            array_adt.print_all()

        elif user_selection == 3:
            search_id = input("Enter Student ID to Search: ").strip()
            found_idx = array_adt.find_index(search_id)
            if found_idx != -1:
                print(f"Record Found:\n{array_adt.fetch(found_idx).info()}")
            else:
                print(f"Record for ID '{search_id}' was not found.")

        elif user_selection == 4:
            update_id = input("Enter Student ID to Update: ").strip()
            if array_adt.find_index(update_id) == -1:
                print("Student ID not found.")
                continue

            updated_name = input("Enter New Name: ").strip()
            updated_course = input("Enter New Course: ").strip()
            updated_year = get_valid_number("Enter New Year Level: ")

            array_adt.edit_student(update_id, updated_name, updated_course, updated_year)
            print("Student record updated successfully!")

        elif user_selection == 5:
            remove_id = input("Enter Student ID to Remove: ").strip()
            if array_adt.delete_student(remove_id):
                print("Student record removed successfully.")
            else:
                print("Student ID not found.")

        elif user_selection == 6:
            print("\n--- DYNAMIC ARRAY METRICS ---")
            print(f"Total Student Count: {array_adt.get_length()}")
            print(f"Current Array Capacity: {array_adt.get_capacity()}")

        elif user_selection == 7:
            print("Exiting Student Record Manager. Program terminated.")
            break

        else:
            print("Option invalid! Pick between 1 and 7.")


# PART 2: MUSIC PLAYLIST MANAGER (SINGLY LINKED LIST ADT)

class Track:
    def __init__(self, track_id: str, title: str, performer: str, duration: str):
        self.track_id = track_id
        self.title = title
        self.performer = performer
        self.duration = duration

    def details(self) -> str:
        return f"[{self.track_id}] {self.title} - {self.performer} ({self.duration})"


class TrackNode:
    def __init__(self, track: Track):
        self.track = track
        self.next_node = None


class SinglyLinkedListADT:
    def __init__(self):
        self.first_node = None
        self.total_nodes = 0

    def check_empty(self) -> bool:
        return self.first_node is None

    def get_total(self) -> int:
        return self.total_nodes

    def add_head(self, track: Track):
        new_entry = TrackNode(track)
        new_entry.next_node = self.first_node
        self.first_node = new_entry
        self.total_nodes += 1

    def add_tail(self, track: Track):
        new_entry = TrackNode(track)
        if self.check_empty():
            self.first_node = new_entry
        else:
            ptr = self.first_node
            while ptr.next_node is not None:
                ptr = ptr.next_node
            ptr.next_node = new_entry
        self.total_nodes += 1

    def add_at_index(self, pos: int, track: Track) -> bool:
        if pos < 1 or pos > self.total_nodes + 1:
            return False

        if pos == 1:
            self.add_head(track)
            return True

        new_entry = TrackNode(track)
        ptr = self.first_node
        for _ in range(1, pos - 1):
            ptr = ptr.next_node

        new_entry.next_node = ptr.next_node
        ptr.next_node = new_entry
        self.total_nodes += 1
        return True

    def render_list(self):
        if self.check_empty():
            print("Playlist is currently empty.")
            return

        print("\n=== PLAYLIST TRACKS ===")
        ptr = self.first_node
        step = 1
        while ptr is not None:
            print(f"{step}. {ptr.track.details()}")
            ptr = ptr.next_node
            step += 1
        print(f"Total Tracks: {self.total_nodes}")

    def find_track(self, track_id: str):
        ptr = self.first_node
        while ptr is not None:
            if ptr.track.track_id.strip().lower() == track_id.strip().lower():
                return ptr.track
            ptr = ptr.next_node
        return None

    def remove_by_id(self, track_id: str) -> bool:
        if self.check_empty():
            return False

        if self.first_node.track.track_id.strip().lower() == track_id.strip().lower():
            self.first_node = self.first_node.next_node
            self.total_nodes -= 1
            return True

        ptr = self.first_node
        while ptr.next_node is not None and ptr.next_node.track.track_id.strip().lower() != track_id.strip().lower():
            ptr = ptr.next_node

        if ptr.next_node is not None:
            ptr.next_node = ptr.next_node.next_node
            self.total_nodes -= 1
            return True

        return False


def get_valid_int(msg: str) -> int:
    while True:
        try:
            return int(input(msg).strip())
        except ValueError:
            print("Invalid input. Please enter a valid number.")


def capture_track_inputs(sll: SinglyLinkedListADT):
    id_input = input("Enter Song ID: ").strip()
    if sll.find_track(id_input) is not None:
        print("Error: Song ID already exists in playlist.")
        return None

    title_input = input("Enter Song Title: ").strip()
    artist_input = input("Enter Artist: ").strip()
    time_input = input("Enter Duration (e.g., 4:23): ").strip()

    return Track(id_input, title_input, artist_input, time_input)


def run_playlist_manager():
    playlist = SinglyLinkedListADT()

    while True:
        print("\n---------------------------------")
        print("     MUSIC PLAYLIST MANAGER      ")
        print("---------------------------------")
        print("1. Add Song at Beginning")
        print("2. Add Song at End")
        print("3. Insert Song at Position")
        print("4. Display Playlist")
        print("5. Search Song")
        print("6. Remove Song")
        print("7. Display Playlist Size")
        print("8. Exit")

        user_choice = get_valid_int("Enter your choice: ")

        if user_choice == 1:
            track = capture_track_inputs(playlist)
            if track:
                playlist.add_head(track)
                print("Song inserted at the head.")

        elif user_choice == 2:
            track = capture_track_inputs(playlist)
            if track:
                playlist.add_tail(track)
                print("Song appended to the end.")

        elif user_choice == 3:
            loc = get_valid_int(f"Enter target position (1 to {playlist.get_total() + 1}): ")
            if loc < 1 or loc > playlist.get_total() + 1:
                print("Invalid target index!")
                continue

            track = capture_track_inputs(playlist)
            if track:
                playlist.add_at_index(loc, track)
                print(f"Song inserted at index {loc}.")

        elif user_choice == 4:
            playlist.render_list()

        elif user_choice == 5:
            s_id = input("Enter Song ID to search: ").strip()
            result = playlist.find_track(s_id)
            if result:
                print(f"Track Found: {result.details()}")
            else:
                print(f"Track ID '{s_id}' was not found.")

        elif user_choice == 6:
            s_id = input("Enter Song ID to remove: ").strip()
            if playlist.remove_by_id(s_id):
                print("Song removed successfully.")
            else:
                print("Song ID not found.")

        elif user_choice == 7:
            print(f"Total Songs in Playlist: {playlist.get_total()}")

        elif user_choice == 8:
            print("Exiting Music Playlist Manager. Goodbye!")
            break

        else:
            print("Option invalid! Pick between 1 and 8.")


if __name__ == "__main__":
    run_student_manager()
    # run_playlist_manager()