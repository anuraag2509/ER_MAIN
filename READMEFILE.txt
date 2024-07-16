Basic Terrain and Landscape using Unity
                                                            ANURAAG RAVI 1NT21IS035

Project Overview
This project demonstrates the creation of a basic terrain and landscape using the Unity software. It includes a traversable environment with trees, grass, flowers, and bushes. The project serves as a baseline for game development or basic scenery rendering, and it can be extended for AR projects or other applications.
Table of Contents
Introduction
Objective
Methodology
Assets Used
Step-by-Step Guide
C# Code
Result Analysis
Introduction
This project models a basic landscape using Unity. The terrain includes trees, grass, flowers, and bushes integrated into a single traversable environment. Unity is a versatile cross-platform software used for developing games and extended reality projects.
Objective
The main objective of this project is to create a virtual environment using Unity. The requirements include creating a landscape/environment and enabling movement or traversal within this environment.
Methodology
Terrain Development: Start by developing the basic terrain.
Asset Import: Import assets such as grass, trees, and flowers from the Unity Asset Store.
Placement: Place the assets in the environment based on required density and aesthetics.
Movement: Write a C# script to facilitate first-person (FPP) or third-person (TPP) movement within the environment.
Assets Used
Trees and Conifers: Conifers BOTD
Terrain: Outdoor Ground Textures
Grass and Flowers: Grass and Flowers Pack
Step-by-Step Guide
Create Terrain:
Open Unity and create a plane in the hierarchy section based on the desired size.
Download Assets:
Download required assets from the Unity Asset Store.
Import Assets:
Use the Package Manager to import assets into the project.
Configure and Place Assets:
Configure each asset in the inspector section and place them accordingly on the plane.
Create Movement Object:
Create an object, character, or reference point for movement within the environment.
Camera and Movement Script:
Pin the camera to the reference object and create a C# script for object movement.
Render and Play:
Render the scene and play to produce the output.
C# Code
csharp
Copy code
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[RequireComponent(typeof(CharacterController))]
public class PlayerMovement : MonoBehaviour
{
    public Camera playerCamera;
    public float walkSpeed = 6f;
    public float runSpeed = 12f;
    public float jumpPower = 7f;
    public float gravity = 10f;
    public float lookSpeed = 2f;
    public float lookXLimit = 45f;
    public float defaultHeight = 2f;
    public float crouchHeight = 1f;
    public float crouchSpeed = 3f;

    private Vector3 moveDirection = Vector3.zero;
    private float rotationX = 0;
    private CharacterController characterController;

    private bool canMove = true;

    void Start()
    {
        characterController = GetComponent<CharacterController>();
        Cursor.lockState = CursorLockMode.Locked;
        Cursor.visible = false;
    }

    void Update()
    {
        Vector3 forward = transform.TransformDirection(Vector3.forward);
        Vector3 right = transform.TransformDirection(Vector3.right);

        bool isRunning = Input.GetKey(KeyCode.LeftShift);
        float curSpeedX = canMove ? (isRunning ? runSpeed : walkSpeed) * Input.GetAxis("Vertical") : 0;
        float curSpeedY = canMove ? (isRunning ? runSpeed : walkSpeed) * Input.GetAxis("Horizontal") : 0;
        float movementDirectionY = moveDirection.y;
        moveDirection = (forward * curSpeedX) + (right * curSpeedY);

        if (Input.GetButton("Jump") && canMove && characterController.isGrounded)
        {
            moveDirection.y = jumpPower;
        }
        else
        {
            moveDirection.y = movementDirectionY;
        }

        if (!characterController.isGrounded)
        {
            moveDirection.y -= gravity * Time.deltaTime;
        }

        if (Input.GetKey(KeyCode.R) && canMove)
        {
            characterController.height = crouchHeight;
            walkSpeed = crouchSpeed;
            runSpeed = crouchSpeed;
        }
        else
        {
            characterController.height = defaultHeight;
            walkSpeed = 6f;
            runSpeed = 12f;
        }

        characterController.Move(moveDirection * Time.deltaTime);

        if (canMove)
        {
            rotationX += -Input.GetAxis("Mouse Y") * lookSpeed;
            rotationX = Mathf.Clamp(rotationX, -lookXLimit, lookXLimit);
            playerCamera.transform.localRotation = Quaternion.Euler(rotationX, 0, 0);
            transform.rotation *= Quaternion.Euler(0, Input.GetAxis("Mouse X") * lookSpeed, 0);
        }
    }
}
Result Analysis
The project successfully met all the objectives. Using the conventional methodology made the entire process easier. The result is a completely traversable environment with a cube as a reference object to demonstrate movement.
