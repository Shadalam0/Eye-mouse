import cv2
import mediapipe as mp
import pyautogui
cam = cv2.VideoCapture(0)
face_mesh = mp.solutions.face_mesh.FaceMesh(refine_landmarks=True)
screen_w, screen_h = pyautogui.size()
while True:
    _, frame = cam.read()
    frame = cv2.flip(frame, 1)
    rgb_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
    output = face_mesh.process(rgb_frame)
    landmark_points = output.multi_face_landmarks
    frame_h, frame_w, _ = frame.shape
    if landmark_points:
        landmarks = landmark_points[0].landmark
        for id, landmark in enumerate(landmarks[474:476]): #for mouse movement number 474:478
           # print(len(landmarks[474:476])) #for mouse movement number 474:478
            x = int(landmark.x * frame_w)
            y = int(landmark.y * frame_h)
            cv2.circle(frame, (x, y), 3, (0, 255, 0))
            if id == 1:
                screen_x = 1.5*screen_w/frame_w * x
                screen_y = 1.5*screen_h/frame_h * y
                pyautogui.moveTo(screen_x, screen_y)
        left = [landmarks[145], landmarks[159]]
        for landmark in left:
            x = int(landmark.x * frame_w)
            y = int(landmark.y * frame_h)
            cv2.circle(frame, (x, y), 3, (0, 255, 255))
       # print(left[0].y - left[1].y)
        if (left[0].y - left[1].y) < 0.008:
           # print('click')
            pyautogui.click()
            pyautogui.sleep(0.2)
        #for right click
        right = [landmarks[380], landmarks[385]]
        for landmark in right:
            x = int(landmark.x * frame_w)
            y = int(landmark.y * frame_h)
            cv2.circle(frame, (x, y), 3, (255, 0, 255))
       # print(right[0].y - right[1].y)
        if (right[0].y - right[1].y) < 0.008:
            #print('right-click')
            pyautogui.rightClick()
            pyautogui.sleep(0.2)
    cv2.imshow('eye mouse', frame)
    cv2.waitKey(1)
