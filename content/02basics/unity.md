---
title: Unity
weight: 1
---

```cs {linenos=table}
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Player : MonoBehaviour
{
    private Rigidbody2D rb;
    public float speed;
    public float angSpeed;
    public GameObject explosionTemplate;
    
    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    // Update is called once per frame
    void Update()
    {
    }

    // FixedUpdate is called once per physics engine round
    void FixedUpdate()
    {
        //Update rotation
        rb.MoveRotation(rb.rotation - Input.GetAxis("Horizontal") * angSpeed * Time.fixedDeltaTime);

        //Update position
        Vector3 pos = transform.position;
        float y = Input.GetAxis("Vertical") * speed * Time.deltaTime;

        Vector2 velocityChange = new Vector3(0, y);
        velocityChange = this.transform.rotation * velocityChange;
        rb.velocity += velocityChange;

        //Handle screen wrapping
        int margin = 50;
        Vector2 screenPos = Camera.main.WorldToScreenPoint(this.transform.position);
        if( screenPos.x > Screen.width+margin ){
            //off right, move to left
            screenPos.x = -margin;
        }
        if( screenPos.x < -margin ){
            //off left, move to right
            screenPos.x = Screen.width+margin;
        }
        if( screenPos.y > Screen.height+margin ){
            //off right, move to left
            screenPos.y = -margin;
        }
        if( screenPos.y < -margin ){
            //off left, move to right
            screenPos.y = Screen.height+margin;
        }

        Vector3 newWorldPosition = Camera.main.ScreenToWorldPoint(screenPos);
        this.transform.position = new Vector2(newWorldPosition.x, newWorldPosition.y);
    }

    public void OnCollisionEnter2D(Collision2D other)
    {
        if( other.gameObject.CompareTag("Rock") || other.gameObject.CompareTag("RockSmall") ){
            this.gameObject.SetActive(false);
            GameObject explosion = Instantiate(explosionTemplate,
                this.transform.position, this.transform.rotation)
                as GameObject;
        }
    }

    public void OnTriggerEnter2D(Collider2D other)
    {
        if( other.gameObject.CompareTag("Token") ){
            Destroy(other.gameObject);
        }

    }
}
```